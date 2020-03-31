---
title: 'Referencia: Data Science Virtual Machine para CentOS'
titleSuffix: Azure Data Science Virtual Machine
description: Detalles de las herramientas incluidas en la máquina Data Science Virtual Machine de CentOS
author: gvashishtha
ms.service: machine-learning
ms.subservice: data-science-vm
ms.author: gopalv
ms.date: 09/11/2019
ms.topic: reference
ms.openlocfilehash: db49a9f5e0e6675d93cb58d6af9c92fac21e8b74
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77525845"
---
# <a name="reference-centos-linux-data-science-virtual-machine"></a>Referencia: Data Science Virtual Machine de CentOS (Linux)

Data Science Virtual Machine (DSVM) de Linux es una máquina virtual de Azure basada en CentOS. DSVM de Linux incluye una colección de herramientas preinstaladas que se pueden usar para el análisis de datos y el aprendizaje automático. 

Los componentes de software principales incluidos en una instancia de DSVM de Linux son:

* Sistema operativo de distribución de Linux CentOS
* Microsoft Machine Learning Server
* Anaconda Python Distribution (versiones 3.5 y 2.7), incluidas las bibliotecas de análisis de datos más conocidas
* JuliaPro, distribución protegida del lenguaje Julia y bibliotecas científicas y de análisis de datos conocidas
* Instancia independiente de Spark y Hadoop de un solo nodo (HDFS, YARN)
* JupyterHub, un servidor de Jupyter Notebook multiusuario compatible con kernels de R, Python, PySpark y Julia
* Explorador de Azure Storage
* CLI de Azure, la interfaz de la línea de comandos de Azure para administrar recursos de Azure
* PostgresSQL Database
* Herramientas de aprendizaje automático:
  * [Microsoft Cognitive Toolkit](https://github.com/Microsoft/CNTK) (CNTK): kit de herramientas de software de aprendizaje profundo de Microsoft Research.
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit), sistema de aprendizaje automático rápido que admite varias técnicas, como el aprendizaje en línea, el uso de hash, la clase AllReduce, las reducciones, learning2search y el aprendizaje activo e interactivo.
  * [XGBoost](https://xgboost.readthedocs.org/en/latest/), herramienta que proporciona implementación de árbol ampliada rápida y precisa.
  * [Rattle](https://togaware.com/rattle/), herramienta que facilita los inicios con el análisis de datos y el aprendizaje automático en R. Rattle ofrece exploración y modelado de datos basados en GUI mediante la generación automática de código R.
* SDK de Azure en Java, Python, Node.js, Ruby y PHP.
* Bibliotecas en R y Python para usarlas en Azure Machine Learning y otros servicios de Azure.
* Editores y herramientas de desarrollo (RStudio, PyCharm, IntelliJ, Emacs, gedit, vi)

La ciencia de datos implica la iteración de una secuencia de tareas:

1. Búsqueda, carga y preprocesamiento de datos
1. Compile y pruebe los modelos.
1. Implemente los modelos para el consumo en aplicaciones inteligentes.

Los científicos de datos usan varias herramientas para realizar estas tareas. Encontrar las versiones de software adecuadas y luego descargarlas, compilarlas e instalarlas puede resultar lento.

DSVM de Linux puede aliviar esta carga de forma considerable. Use DSVM de Linux para iniciar el proyecto de análisis. DSVM de Linux le permite trabajar en tareas en varios lenguajes, incluidos R, Python, SQL, Java y C++. Eclipse proporciona un entorno de desarrollo integrado (IDE) fácil de usar para el desarrollo y las pruebas de código. El SDK de Azure incluido en DSVM ayuda a compilar las aplicaciones gracias al uso de varios servicios en Linux para la plataforma en la nube de Microsoft. Otros lenguajes, como Ruby, Perl, PHP y Node.js, están preinstalados.

No hay cargos de software por la imagen de DSVM. Solo se pagan las cuotas de uso del hardware de Azure, que se calculan en función del tamaño de la máquina virtual que se aprovisiona con la imagen de DSVM. Para más información sobre las cuotas de proceso, consulte la [lista de Data Science Virtual Machine para Linux (CentOS)](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/) en Azure Marketplace.


## <a name="machine-learning-server"></a>Machine Learning Server

R es uno de los lenguajes más conocidos para el análisis de datos y el aprendizaje automático. Si quiere usar R para su análisis, la instancia de DSVM incluye Machine Learning Server con Microsoft R Open y la biblioteca Math Kernel Library. Math Kernel Library optimiza las operaciones matemáticas comunes en algoritmos analíticos. R Open es totalmente compatible con CRAN-R; cualquiera de las bibliotecas de R publicadas en CRAN puede instalarse en R Open. 

Puede usar Machine Learning Server para escalar y poner en funcionamiento los modelos de R en servicios web. Puede editar los programas de R en uno de los editores predeterminados, como RStudio, vi o Emacs. El editor de Emacs está preinstalado en DSVM. El paquete de Emacs ESS (Emacs Speaks Statistics) simplifica el trabajo con los archivos de R en el editor Emacs.

Para abrir la consola de R, en el shell, escriba **R**. Este comando lo dirigirá a un entorno interactivo. Para desarrollar el programa de R, normalmente se usa un editor como Emacs o vi y, luego, se ejecutan los scripts en R. RStudio ofrece un IDE gráfico completo para desarrollar el programa de R.

En DSVM se incluye un script de R que puede usar para instalar los [20 primeros paquetes de R](https://www.kdnuggets.com/2015/06/top-20-r-packages.html). Puede ejecutar este script cuando esté en la interfaz interactiva de R. Como se mencionó anteriormente, para abrir esa interfaz, en el shell, escriba **R**.  

## <a name="python"></a>Python

Anaconda Python se instala con los entornos Python 3.5 y 2.7. El entorno 2.7 se denomina _root_ y el entorno 3.5 se denomina _py35_. Esta distribución contiene Python base, junto con aproximadamente 300 de los paquetes de matemáticas, ingeniería y análisis de datos más populares.

El entorno py35 es el predeterminado. Para activar el entorno root (2.7), use este comando:

```bash
source activate root
```

Para volver a activar el entorno py35, use este comando:

```bash
source activate py35
```

Para invocar la sesión interactiva de Python, escriba **python** en el shell. 

Instale bibliotecas de Python adicionales mediante Conda o pip. Para pip, active primero el entorno correcto si no quiere el predeterminado:

```bash
source activate root
pip install <package>
```

O bien, especifique la ruta de acceso completa a pip:

```bash
/anaconda/bin/pip install <package>
```

En el caso de Conda, siempre debe especificar el nombre del entorno (py35 o root):

```bash
conda install <package> -n py35
```

Si se encuentra en una interfaz gráfica o tiene la configuración de reenvío de X11, puede escribir **pycharm** para abrir el IDE de PyCharm Python. Puede usar los editores de texto predeterminados. Además, puede usar Spyder, un IDE de Python que se incluye con las distribuciones de Anaconda Python. Spyder necesita un escritorio gráfico o el reenvío de X11. El escritorio gráfico tiene un acceso directo a Spyder.

## <a name="jupyter-notebook"></a>Jupyter Notebook

La distribución de Anaconda también incluye una instancia de Jupyter Notebook, un entorno para compartir código y análisis. Acceda a Jupyter Notebook a través de JupyterHub. Inicie sesión con su nombre de usuario y contraseña de Linux local.

El servidor de Jupyter Notebook se configura previamente con kernels de Python 2, Python 3 y R. Use el icono de escritorio de **Jupyter Notebook** para abrir el explorador y acceder al servidor de Jupyter Notebook. Si accede a la instancia de DSVM a través de SSH o del cliente X2Go, también puede acceder al servidor de Jupyter Notebook en https:\//localhost:8000/.

> [!NOTE]
> Continúe aunque aparezca una advertencia de certificado.

Puede acceder al servidor de Jupyter Notebook desde cualquier host. Escriba **https:\//\<Nombre DNS o dirección IP de DSVM\>:8000/** .

> [!NOTE]
> El puerto 8000 se abre en el firewall de forma predeterminada cuando se aprovisiona la instancia de DSVM. 

Microsoft ha empaquetado algunos cuadernos de ejemplo, uno en Python y otro en R. Puede ver el vínculo a los ejemplos en la página principal de Jupyter Notebook después de autenticarse en Jupyter Notebook con el nombre de usuario y la contraseña de Linux local. Para crear un cuaderno, seleccione **New** (Nuevo) y, luego, seleccione el kernel de lenguaje que quiere usar. Si no ve el botón **New** (Nuevo), seleccione el icono **Jupyter** en la parte superior izquierda para ir a la página principal del servidor de cuadernos.

## <a name="spark-standalone"></a>Modo independiente de Spark 

Una instancia del modo independiente de Spark está preinstalada en la instancia de DSVM de Linux para ayudarle a desarrollar aplicaciones de Spark localmente antes de probarlas e implementarlas en clústeres de gran tamaño. 

Puede ejecutar programas PySpark mediante el kernel de Jupyter. Al abrir Jupyter, seleccione el botón **New** (Nuevo); verá una lista de los kernels disponibles. **Spark - Python** es el kernel de PySpark que le permite compilar aplicaciones Spark mediante el lenguaje Python. También puede usar un IDE de Python, como PyCharm o Spyder, para compilar el programa Spark. 

En esta instancia independiente, la pila de Spark se ejecuta en el programa cliente que realiza la llamada. Esta característica permite simplificar y acelerar la solución de problemas en comparación con el desarrollo en un clúster de Spark.

Jupyter proporciona un cuaderno de PySpark de ejemplo. Puede encontrarlo en el directorio SparkML en el directorio principal de Jupyter ($HOME/notebooks/SparkML/pySpark). 

Si va a programar en R para Spark, puede usar Machine Learning Server, SparkR o sparklyr. 

Antes de realizar la ejecución en el contexto de Spark en Machine Learning Server, debe llevar a cabo un paso de configuración una sola vez a fin de habilitar un HDFS de Hadoop de un solo nodo y una instancia de YARN. De manera predeterminada, los servicios de Hadoop están instalados pero deshabilitados en la DSVM. Para habilitar los servicios de Hadoop, ejecute los siguientes comandos como raíz la primera vez:

```bash
echo -e 'y\n' | ssh-keygen -t rsa -P '' -f ~hadoop/.ssh/id_rsa
cat ~hadoop/.ssh/id_rsa.pub >> ~hadoop/.ssh/authorized_keys
chmod 0600 ~hadoop/.ssh/authorized_keys
chown hadoop:hadoop ~hadoop/.ssh/id_rsa
chown hadoop:hadoop ~hadoop/.ssh/id_rsa.pub
chown hadoop:hadoop ~hadoop/.ssh/authorized_keys
systemctl start hadoop-namenode hadoop-datanode hadoop-yarn
```

Puede detener los servicios relacionados con Hadoop cuando no los necesite si ejecuta `systemctl stop hadoop-namenode hadoop-datanode hadoop-yarn`.

El directorio /dsvm/samples/MRS proporciona un ejemplo que muestra cómo desarrollar y probar Machine Learning Server en un contexto de Spark remoto (la instancia de Spark independiente en DSVM).

## <a name="ides-and-editors"></a>IDE y editores

Puede elegir entre varios editores de código, como vi/VIM, Emacs, gedit, PyCharm, RStudio, Eclipse, LaTeX e IntelliJ. 

* gedit, Eclipse, IntelliJ, R Studio y PyCharm son editores gráficos. Para usarlos, debe haber iniciado sesión en un escritorio gráfico. Se abren mediante accesos directos de menú de escritorio y aplicación.

* Vim y Emacs son editores basados en texto. En Emacs, el paquete de complementos de ESS facilita el trabajo con R en el editor Emacs. Puede encontrar más información en el [sitio web de ESS](https://ess.r-project.org/).

* Eclipse es un IDE extensible de código abierto que admite varios lenguajes. El IDE de Eclipse para desarrolladores de Java es la versión instalada en la instancia de DSVM. Puede instalar complementos de varios lenguajes conocidos para ampliar el entorno. 

  El complemento Azure Toolkit for Eclipse se instala con Eclipse en la instancia de DSVM. Puede usarlo para crear, desarrollar, probar e implementar aplicaciones de Azure mediante el entorno de desarrollo Eclipse que admite lenguajes como Java.

  El SDK de Azure para Java también se instala con el complemento Azure Toolkit for Eclipse en la instancia de DSVM. Este SDK le proporciona acceso a distintos servicios de Azure dentro de un entorno de Java. 
  
  Para obtener más información, consulte [Kit de herramientas de Azure para Eclipse](/java/azure/eclipse/azure-toolkit-for-eclipse).

* LaTeX se instala mediante el paquete texlive, junto con un paquete de complementos de Emacs llamado [AUCTeX](https://www.gnu.org/software/auctex/manual/auctex/auctex.html). Este paquete simplifica la creación de documentos de LaTeX en Emacs. 

## <a name="databases"></a>Bases de datos

La instancia de DSVM de Linux ofrece acceso a varias herramientas de base de datos y de línea de comandos.

### <a name="postgressql"></a>PostgresSQL

La base de datos de código abierto PostgresSQL está disponible en la instancia de DSVM, con servicios que se ejecutan y con initdb ya completado. Debe crear bases de datos y usuarios. Para más información, consulte la [documentación de PostgresSQL](https://www.postgresql.org/docs/).  

### <a name="squirrel-sql"></a>SQuirreL SQL

SQuirreL SQL es un cliente SQL gráfico que se puede conectar a varias bases de datos (incluidas SQL Server, PostgresSQL y MySQL) y ejecutar consultas SQL. Puede ejecutar SQuirreL SQL desde una sesión de escritorio gráfico (a través del cliente de X2Go, por ejemplo) mediante un icono de escritorio. O bien, puede ejecutar el cliente mediante el comando siguiente en el shell:

```bash
/usr/local/squirrel-sql-3.7/squirrel-sql.sh /usr/local/squirrel-sql-3.7/squirrel-sql.sh
```

Antes de usarlo por primera vez, configure los controladores y los alias de las bases de datos. Los controladores JDBC se encuentran en /usr/share/java/jdbcdrivers.

Para más información, consulte [SQuirreL SQL](http://squirrel-sql.sourceforge.net/index.php?page=screenshots).

### <a name="command-line-tools-for-accessing-sql-server"></a>Herramientas de línea de comandos para acceder a SQL Server

El paquete de controladores ODBC para SQL Server también incluye dos herramientas de línea de comandos:

* **bcp**: la herramienta bcp copia datos de forma masiva entre una instancia de SQL Server y un archivo de datos en un formato especificado por el usuario. Puede usarla para importar grandes cantidades de filas nuevas en tablas de SQL Server o exportar datos de tablas en archivos de datos. Para importar datos en una tabla, debe usar un archivo de formato creado para esa tabla. O bien, debe comprender la estructura de la tabla y los tipos de datos que son válidos para sus columnas.

  Para más información, consulte [Conexión con bcp](https://msdn.microsoft.com/library/hh568446.aspx).

* **sqlcmd**: la utilidad sqlcmd se puede usar para insertar instrucciones Transact-SQL, procedimientos del sistema y archivos de script en el símbolo del sistema. Esta utilidad usa ODBC para ejecutar lotes de Transact-SQL.

  Para más información, consulte [Conexión con sqlcmd](https://msdn.microsoft.com/library/hh568447.aspx).

  > [!NOTE]
  > Existen algunas diferencias en esta herramienta entre las plataformas Windows y Linux. Consulte la documentación de para obtener más información.

### <a name="database-access-libraries"></a>Bibliotecas de acceso a las bases de datos

Las bibliotecas de acceso a las bases de datos están disponibles en R y Python:

* En R, puede usar el paquete RODBC o el paquete dplyr para consultar o ejecutar instrucciones SQL en el servidor de bases de datos.
* En Python, la biblioteca pyodbc proporciona acceso a las bases de datos con ODBC como capa subyacente.

## <a name="azure-tools"></a>Herramientas de Azure

Las siguientes herramientas se instalan en la instancia de DSVM:

* **CLI de Azure**: puede usar la interfaz de la línea de comandos de Azure para crear y administrar recursos de Azure mediante comandos de shell. Para abrir las herramientas de Azure, escriba **azure help**. Para obtener más información, consulte la [página de documentación de la CLI de Azure](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).
* **Explorador de Azure Storage** el Explorador de Azure Storage es una herramienta gráfica que se usa para examinar los objetos que se han almacenado en la cuenta de Azure Storage y para cargar y descargar datos con los blobs de Azure como origen y destino. Puede acceder al Explorador de Storage desde el icono de acceso directo del escritorio. También puede abrirlo desde un símbolo del sistema del shell si escribe **StorageExplorer**. Debe haber iniciado sesión desde un cliente X2Go o tener la configuración de reenvío X11.
* **Bibliotecas de Azure**: las bibliotecas siguientes están preinstaladas en la instancia de DSVM:
  
  * **Python**: las bibliotecas relacionadas con Azure en Python son *azure*, *azureml*, *pydocumentdb* y *pyodbc*. Las tres primeras bibliotecas permiten acceder a los servicios de Azure Storage, a Azure Machine Learning y a Azure Cosmos DB (una base de datos NoSQL en Azure). La cuarta biblioteca, pyodbc (junto con el controlador ODBC de Microsoft para SQL Server), permite el acceso a SQL Server, Azure SQL Database y Azure SQL Data Warehouse desde Python mediante una interfaz ODBC. Escriba **pip list** para ver todas las bibliotecas enumeradas. Asegúrese de ejecutar este comando en los entornos de Python 2.7 y 3.5.
  * **R**: las bibliotecas relacionadas con Azure en R son AzureML y RODBC.
  * **Java**: la lista de bibliotecas de Java para Azure se puede encontrar en el directorio /dsvm/sdk/AzureSDKJava de la instancia de DSVM. Las bibliotecas principales son Azure Storage y las API de administración, Azure Cosmos DB y los controladores JDBC para SQL Server.  

Puede acceder a [Azure Portal](https://portal.azure.com) desde el explorador Firefox instalado previamente. En Azure Portal, puede crear, administrar y supervisar los recursos de Azure.

## <a name="azure-machine-learning"></a>Azure Machine Learning

Azure Machine Learning es un servicio en la nube totalmente administrado que permite crear, implementar y compartir soluciones de análisis predictivo. Puede crear sus experimentos y modelos desde Azure Machine Learning Studio (clásico). Para acceder a Azure Machine Learning desde un explorador web en la instancia de DSVM, vaya a [Microsoft Azure Machine Learning](https://studio.azureml.net).

Después de iniciar sesión en Azure Machine Learning Studio (clásico), puede usar un lienzo de experimentación para crear un flujo lógico para los algoritmos de aprendizaje automático. También tiene acceso a una instancia de Jupyter Notebook que se hospeda en Azure Machine Learning. El cuaderno puede funcionar sin problemas con los experimentos de Azure Machine Learning Studio (clásico). 

Encapsule los modelos de aprendizaje automático que ha creado en una interfaz de servicio web para que estén operativos. El funcionamiento de modelos de aprendizaje automático permite a los clientes escritos en cualquier lenguaje invocar predicciones a partir de esos modelos. Para obtener más información, consulte la [documentación sobre Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/).

También puede crear los modelos en R o en Python en la instancia de DSVM y, después, implementarlos en producción en Azure Machine Learning. Microsoft ha instalado bibliotecas en R (**AzureML**) y en Python (**azureml**) para admitir esta funcionalidad.

Para información sobre cómo implementar modelos en R y Python en Azure Machine Learning, consulte [Diez cosas que puede hacer en Windows Data Science Virtual Machine](vm-do-ten-things.md).

> [!NOTE]
> Las [instrucciones de este artículo](vm-do-ten-things.md) se escribieron para la versión de Windows de la instancia de DSVM. Sin embargo, la información sobre la implementación de modelos en Azure Machine Learning también se aplica a la instancia de DSVM de Linux.

## <a name="machine-learning-tools"></a>Herramientas de aprendizaje automático

La instancia de DSVM incluye algunas herramientas o algoritmos de aprendizaje automático que se han pregenerado y preinstalado de forma local. Entre ellas se incluyen las siguientes:

* **Microsoft Cognitive Toolkit**: un kit de herramientas de aprendizaje profundo.
* **Vowpal Wabbit**: algoritmo de aprendizaje rápido en línea.
* **XGBoost**: herramienta que proporciona los algoritmos de árbol ampliados y optimizados.
* **Python**: Anaconda Python integra algoritmos de aprendizaje automático con bibliotecas como Scikit-learn. Puede instalar otras bibliotecas con el comando `pip install` .
* **R**: una amplia biblioteca de funciones de aprendizaje automático está disponible para R. Las bibliotecas preinstaladas incluyen lm, glm, randomForest y rpart. Puede instalar otras bibliotecas mediante la ejecución de `install.packages(<lib name>)`.

Microsoft Cognitive Toolkit, Vowpal Wabbit y XGBoost se describen con más detalle en las secciones siguientes.

### <a name="microsoft-cognitive-toolkit"></a>Microsoft Cognitive Toolkit

Microsoft Cognitive Toolkit es un kit de herramientas de aprendizaje profundo de código abierto. Es una herramienta de línea de comandos (CNTK) y ya se encuentra en PATH.

Para ver un ejemplo básico, ejecute los comandos siguientes en el shell:

```bash
cd /home/[USERNAME]/notebooks/CNTK/HelloWorld-LogisticRegression
cntk configFile=lr_bs.cntk makeMode=false command=Train
```

Para más información, consulte el [repositorio de CNTK de GitHub](https://github.com/Microsoft/CNTK) y la [wiki de CNTK](https://github.com/Microsoft/CNTK/wiki).

### <a name="vowpal-wabbit"></a>Vowpal Wabbit

Vowpal Wabbit es un sistema de aprendizaje automático que emplea técnicas, como el aprendizaje en línea, el uso de hash, la clase AllReduce, las reducciones, learning2search y el aprendizaje activo e interactivo.

Para ejecutar la herramienta en un ejemplo básico, ejecute los siguientes comandos:

```bash
cp -r /dsvm/tools/VowpalWabbit/demo vwdemo
cd vwdemo
vw house_dataset
```

El directorio de demostración de Vowpal Wabbit incluye otras demostraciones más grandes. Para más información sobre Vowpal Wabbit, consulte el [repositorio de Vowpal Wabbit de GitHub](https://github.com/JohnLangford/vowpal_wabbit) y la [wiki de Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit/wiki).

### <a name="xgboost"></a>XGBoost

Se trata de una biblioteca que está diseñada y optimizada para algoritmos potenciados (árbol). El objetivo de esta biblioteca es llevar los límites del cálculo de las máquinas a los extremos necesarios para proporcionar un incremento en árbol a gran escala que sea escalable, portátil y preciso.

XGBoost se proporciona como una línea de comandos y como una biblioteca de R.

Para usar la biblioteca XGBoost en R, inicie una sesión interactiva de R (en el shell, escriba **R**) y, luego, cargue la biblioteca.

Este es un ejemplo sencillo que puede ejecutar en el símbolo del sistema de R:

```R
library(xgboost)

data(agaricus.train, package='xgboost')
data(agaricus.test, package='xgboost')
train <- agaricus.train
test <- agaricus.test
bst <- xgboost(data = train$data, label = train$label, max.depth = 2,
                eta = 1, nthread = 2, nround = 2, objective = "binary:logistic")
pred <- predict(bst, test$data)
```

Para ejecutar la línea de comandos de XGBoost, ejecute estos comandos en el shell:

```bash
cp -r /dsvm/tools/xgboost/demo/binary_classification/ xgboostdemo
cd xgboostdemo
xgboost mushroom.conf
```

En el directorio especificado se escribe un archivo .model. Para información sobre este ejemplo de demostración en GitHub, consulte la [clasificación binaria](https://github.com/dmlc/xgboost/tree/master/demo/binary_classification).

Para más información sobre XGBoost, consulte la [documentación de XGBoost](https://xgboost.readthedocs.org/en/latest/) y el [repositorio de GitHub para XGBoost](https://github.com/dmlc/xgboost).

### <a name="rattle"></a>Rattle

Rattle (*R* *A*nalytical *T*ool *T*o *L*earn *E*asily, la herramienta de análisis de R para aprender fácilmente) utiliza el modelado y la exploración de datos basados en GUI. Rattle:
- Presenta resúmenes estadísticos y visuales de los datos.
- Transforma los datos que se pueden modelar fácilmente.
- Compila los modelos no supervisados y supervisados a partir de los datos.
- Presenta el rendimiento de los modelos gráficamente.
- Puntúa nuevos conjuntos de datos.
- Genera código R.
- Replica las operaciones en la interfaz de usuario que se pueden ejecutar directamente en R o usarse como punto de partida para un análisis posterior.

Para ejecutar Rattle, debe iniciar sesión en una sesión de escritorio gráfico. En un terminal, escriba **R** para abrir el entorno de R. En el símbolo del sistema de R, escriba los siguientes comandos:

```R
library(rattle)
rattle()
```

Se abre una interfaz gráfica que tiene un conjunto de pestañas. Siga estos pasos de inicio rápido en Rattle para usar un conjunto de datos meteorológicos de ejemplo y crear un modelo. En algunos de los pasos, se le solicitará que instale y cargue automáticamente algún paquete de R necesario que todavía no esté en el sistema.

> [!NOTE]
> Si no tiene permisos para instalar el paquete en el directorio del sistema (valor predeterminado), puede que aparezca un aviso en la ventana de la consola de R para instalar paquetes en su biblioteca personal. Si ve estos avisos, escriba **y**.

1. Seleccione **Execute**(Ejecutar).
1. Un cuadro de diálogo le pide que cargue el conjunto de datos meteorológicos de ejemplo. Haga clic en **Yes** (Sí) para cargar el ejemplo.
1. Seleccione la pestaña **Model** (Modelo).
1. Seleccione **Execute** (Ejecutar) para crear un árbol de decisión.
1. Seleccione **Draw** (Dibujar) para mostrar el árbol de decisión.
1. Seleccione la opción **Forest** (Bosque) y, luego, seleccione **Execute** (Ejecutar) para crear un bosque aleatorio.
1. Seleccione la pestaña **Evaluate** (Evaluar).
1. Seleccione la opción **Risk** (Riesgo) y, luego, **Execute** (Ejecutar) para mostrar dos trazados de rendimiento de **Risk (Cumulative)** (Riesgo [acumulativo]).
1. Seleccione la pestaña **Log** (Registro) para mostrar el código R generado con las operaciones anteriores. (Debido a un error de la versión actual de Rattle, debe insertar un carácter **#** delante de **Export this log** [Exportar este registro] en el texto del registro).
1. Seleccione el botón **Export** (Exportar) para guardar el archivo de script de R llamado *weather_script.R* en la carpeta principal.

Puede salir de Rattle y R. Ahora puede modificar el script de R generado. O bien, use el script tal y como está y ejecútelo en cualquier momento para repetir todo lo que se hizo en la interfaz de usuario de Rattle. Esta es una forma sencilla, especialmente para los principiantes en R, de realizar análisis y aprendizaje automático rápidamente en una interfaz gráfica sencilla, al mismo tiempo que se genera automáticamente código en R para modificar o aprender.

## <a name="next-steps"></a>Pasos siguientes

¿Tiene más preguntas? Considere la posibilidad de crear [una incidencia de soporte técnico](https://azure.microsoft.com/support/create-ticket/).