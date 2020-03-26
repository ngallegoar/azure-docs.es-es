---
title: 'Tutorial: Prueba de módulos de Terraform en Azure mediante Terratest'
description: Aprenda a usar Terratest para probar los módulos de Terraform.
ms.topic: tutorial
ms.date: 10/26/2019
ms.openlocfilehash: 687a793af2b9b75efe463b042d121c32f18974d6
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "79370804"
---
# <a name="tutorial-test-terraform-modules-in-azure-using-terratest"></a>Tutorial: Prueba de módulos de Terraform en Azure mediante Terratest

> [!NOTE]
> El código de ejemplo en este artículo no funciona con la versión 0.12 (y superior).

Puede usar módulos de Terraform de Azure para crear componentes reutilizables, que se pueden componer y probar. Los módulos de Terraform incorporan encapsulación, que es útil para implementar procesos de infraestructura como código.

Es importante implementar un control de calidad al crear módulos de Terraform. Desafortunadamente, hay disponible documentación limitada que explique cómo crear pruebas unitarias y pruebas de integración en los módulos de Terraform. Este tutorial presenta una infraestructura de pruebas y los procedimientos recomendados que hemos adoptado al crear nuestros [módulos de Terraform de Azure](https://registry.terraform.io/browse?provider=azurerm).

Hemos examinado las infraestructuras de pruebas más populares y se eligió [Terratest](https://github.com/gruntwork-io/terratest) para probar los módulos de Terraform. Terratest se implementa como una biblioteca de Go. Terratest proporciona una colección de funciones auxiliares y patrones para tareas de prueba de infraestructura comunes, como hacer solicitudes HTTP y utilizar SSH para acceder a una determinada máquina virtual. En la lista siguiente se describen algunas de las principales ventajas de usar Terratest:

- **Proporciona asistentes prácticos para comprobar la infraestructura**. Esta característica resulta útil cuando desea comprobar la infraestructura real en el entorno real.
- **La estructura de carpetas está organizada claramente**. Los casos de prueba están organizados claramente y siguen la [estructura de carpetas del módulo de Terraform estándar](https://www.terraform.io/docs/modules/create.html#standard-module-structure).
- **Todos los casos de prueba están escritos en Go**. La mayoría de los desarrolladores que usan Terraform son desarrolladores de Go. Si es un desarrollador de Go, no tiene que aprender otro lenguaje de programación para utilizar Terratest. Además, las únicas dependencias necesarias para ejecutar casos de prueba en Terratest son Go y Terraform.
- **La infraestructura es muy extensible**. Puede ampliar funciones adicionales a partir de Terratest, incluidas las características específicas de Azure.

## <a name="prerequisites"></a>Prerrequisitos

Este artículo práctico es independiente de la plataforma. Puede ejecutar los ejemplos de código que usamos en este artículo en Windows, Linux o MacOS. 

Antes de comenzar, instale el software siguiente:

- **Lenguaje de programación Go**: los casos de prueba de Terraform están escritos en [Go](https://golang.org/dl/).
- **dep**: [dep](https://github.com/golang/dep#installation) es una herramienta de administración de dependencias para Go.
- **CLI de Azure**: la [CLI de Azure](/cli/azure/install-azure-cli?view=azure-cli-latest) es una herramienta de línea de comandos que puede utilizar para administrar los recursos de Azure. (Terraform admite la autenticación en Azure mediante una entidad de servicio o [la CLI de Azure](https://www.terraform.io/docs/providers/azurerm/authenticating_via_azure_cli.html)).
- **Mage**: usamos el [ejecutable mage](https://github.com/magefile/mage/releases) para mostrarle cómo simplificar la ejecución de los casos de Terratest. 

## <a name="create-a-static-webpage-module"></a>Creación de un módulo de la página web estática

En este tutorial, creará un módulo de Terraform que aprovisiona una página web estática; para ello, se carga un único archivo HTML en un blob de Azure Storage. Este módulo proporciona a los usuarios de todo el mundo acceso a la página web mediante una dirección URL que el módulo devuelve.

> [!NOTE]
> Cree todos los archivos que se describen en esta sección bajo la ubicación [GOPATH](https://github.com/golang/go/wiki/SettingGOPATH).

En primer lugar, cree una carpeta nueva llamada `staticwebpage` bajo la carpeta `src` de GoPath. La estructura general de carpetas de este tutorial se muestra en el ejemplo siguiente. Los archivos marcados con un asterisco `(*)` son el foco principal de esta sección.

```
 📁 GoPath/src/staticwebpage
   ├ 📁 examples
   │   └ 📁 hello-world
   │       ├ 📄 index.html
   │       └ 📄 main.tf
   ├ 📁 test
   │   ├ 📁 fixtures
   │   │   └ 📁 storage-account-name
   │   │       ├ 📄 empty.html
   │   │       └ 📄 main.tf
   │   ├ 📄 hello_world_example_test.go
   │   └ 📄 storage_account_name_unit_test.go
   ├ 📄 main.tf      (*)
   ├ 📄 outputs.tf   (*)
   └ 📄 variables.tf (*)
```

El módulo de la página web estática acepta tres entradas. Las entradas se declaran en `./variables.tf`:

```hcl
variable "location" {
  description = "The Azure region in which to create all resources."
}

variable "website_name" {
  description = "The website name to use to create related resources in Azure."
}

variable "html_path" {
  description = "The file path of the static home page HTML in your local file system."
  default     = "index.html"
}
```

Como hemos mencionado anteriormente en este artículo, este módulo también genera como salida una dirección URL que se declara en `./outputs.tf`:

```hcl
output "homepage_url" {
  value = azurerm_storage_blob.homepage.url
}
```

La lógica principal del módulo aprovisiona cuatro recursos:
- **grupo de recursos**: el nombre del grupo de recursos es la entrada `website_name` seguida de `-staging-rg`.
- **cuenta de almacenamiento**: el nombre de la cuenta de almacenamiento es la entrada `website_name` seguida de `data001`. Para cumplir las limitaciones de nomenclatura de la cuenta de almacenamiento, el módulo quita todos los caracteres especiales y usa letras minúsculas en el nombre de cuenta de almacenamiento.
- **contenedor de nombres corregidos**: el contenedor se llama `wwwroot` y se crea en la cuenta de almacenamiento.
- **archivo HTML único**: el único archivo HTML se lee en la entrada `html_path` y se carga en `wwwroot/index.html`.

La lógica del módulo de la página web estática se implementa en `./main.tf`:

```hcl
resource "azurerm_resource_group" "main" {
  name     = "${var.website_name}-staging-rg"
  location = var.location
}

resource "azurerm_storage_account" "main" {
  name                     = "${lower(replace(var.website_name, "/[[:^alnum:]]/", ""))}data001"
  resource_group_name      = azurerm_resource_group.main.name
  location                 = azurerm_resource_group.main.location
  account_tier             = "Standard"
  account_replication_type = "LRS"
}

resource "azurerm_storage_container" "main" {
  name                  = "wwwroot"
  resource_group_name   = azurerm_resource_group.main.name
  storage_account_name  = azurerm_storage_account.main.name
  container_access_type = "blob"
}

resource "azurerm_storage_blob" "homepage" {
  name                   = "index.html"
  resource_group_name    = azurerm_resource_group.main.name
  storage_account_name   = azurerm_storage_account.main.name
  storage_container_name = azurerm_storage_container.main.name
  source                 = var.html_path
  type                   = "block"
  content_type           = "text/html"
}
```

### <a name="unit-test"></a>Prueba unitaria

Terratest está diseñado para pruebas de integración. Con ese fin, Terratest aprovisiona recursos reales en un entorno real. A veces, los trabajos de las pruebas de integración pueden ser excepcionalmente grandes, en especial cuando se tiene que aprovisionar un gran número de recursos. La lógica que convierte los nombres de cuenta de almacenamiento a los que nos referimos en la sección anterior es un buen ejemplo. 

Sin embargo, realmente no es necesario aprovisionar los recursos. Solo queremos asegurarnos de que la lógica de conversión de nomenclatura es correcta. Gracias a la flexibilidad de Terratest, podemos usar pruebas unitarias. Las pruebas unitarias son casos de prueba de ejecución local (aunque se requiere acceso a Internet). Los casos de pruebas unitarias ejecutan los comandos `terraform init` y `terraform plan` para analizar la salida de `terraform plan` y buscar los valores de atributo para comparar.

El resto de esta sección describe cómo usar Terratest para implementar una prueba unitaria a fin de asegurarse de que la lógica utilizada en la conversión de la nomenclatura de cuentas de almacenamiento es correcta. Solo nos interesan los archivos marcados con un asterisco `(*)`.

```
 📁 GoPath/src/staticwebpage
   ├ 📁 examples
   │   └ 📁 hello-world
   │       ├ 📄 index.html
   │       └ 📄 main.tf
   ├ 📁 test
   │   ├ 📁 fixtures
   │   │   └ 📁 storage-account-name
   │   │       ├ 📄 empty.html                (*)
   │   │       └ 📄 main.tf                   (*)
   │   ├ 📄 hello_world_example_test.go
   │   └ 📄 storage_account_name_unit_test.go (*)
   ├ 📄 main.tf
   ├ 📄 outputs.tf
   └ 📄 variables.tf
```

En primer lugar, se usa un archivo HTML vacío llamado `./test/fixtures/storage-account-name/empty.html` como marcador de posición.

El archivo `./test/fixtures/storage-account-name/main.tf` es el marco del caso de prueba. Acepta una entrada, `website_name`, que también es la entrada de las pruebas unitarias. La lógica se muestra aquí:

```hcl
variable "website_name" {
  description = "The name of your static website."
}

module "staticwebpage" {
  source       = "../../../"
  location     = "West US"
  website_name = var.website_name
  html_path    = "empty.html"
}
```

El componente principal es la implementación de las pruebas unitarias en `./test/storage_account_name_unit_test.go`.

Los desarrolladores de Go probablemente advertirán que la prueba unitaria coincide con la firma de una función de prueba de Go clásica al aceptar un argumento de tipo `*testing.T`.

En el cuerpo de la prueba unitaria, tenemos un total de cinco casos que se definen en la variable `testCases` (`key` como entrada y `value` como salida esperada). Para cada caso de prueba unitaria, se ejecuta en primer lugar `terraform init` y se establece como destino la carpeta del accesorio de prueba (`./test/fixtures/storage-account-name/`). 

A continuación, un comando `terraform plan` que usa la entrada del caso de prueba específico (eche un vistazo a la definición de `website_name` en `tfOptions`) guarda el resultado en `./test/fixtures/storage-account-name/terraform.tfplan` (que no aparece en la estructura general de carpetas).

Este archivo de resultados se analiza en una estructura de código legible mediante el analizador oficial de planes de Terraform.

Ahora, buscamos los atributos que nos interesan (en este caso, el atributo `name` de `azurerm_storage_account`) y se comparan los resultados con la salida esperada:

```go
package test

import (
    "os"
    "path"
    "testing"

    "github.com/gruntwork-io/terratest/modules/terraform"
    terraformCore "github.com/hashicorp/terraform/terraform"
)

func TestUT_StorageAccountName(t *testing.T) {
    t.Parallel()

    // Test cases for storage account name conversion logic
    testCases := map[string]string{
        "TestWebsiteName": "testwebsitenamedata001",
        "ALLCAPS":         "allcapsdata001",
        "S_p-e(c)i.a_l":   "specialdata001",
        "A1phaNum321":     "a1phanum321data001",
        "E5e-y7h_ng":      "e5ey7hngdata001",
    }

    for input, expected := range testCases {
        // Specify the test case folder and "-var" options
        tfOptions := &terraform.Options{
            TerraformDir: "./fixtures/storage-account-name",
            Vars: map[string]interface{}{
                "website_name": input,
            },
        }

        // Terraform init and plan only
        tfPlanOutput := "terraform.tfplan"
        terraform.Init(t, tfOptions)
        terraform.RunTerraformCommand(t, tfOptions, terraform.FormatArgs(tfOptions, "plan", "-out="+tfPlanOutput)...)

        // Read and parse the plan output
        f, err := os.Open(path.Join(tfOptions.TerraformDir, tfPlanOutput))
        if err != nil {
            t.Fatal(err)
        }
        defer f.Close()
        plan, err := terraformCore.ReadPlan(f)
        if err != nil {
            t.Fatal(err)
        }

        // Validate the test result
        for _, mod := range plan.Diff.Modules {
            if len(mod.Path) == 2 && mod.Path[0] == "root" && mod.Path[1] == "staticwebpage" {
                actual := mod.Resources["azurerm_storage_account.main"].Attributes["name"].New
                if actual != expected {
                    t.Fatalf("Expect %v, but found %v", expected, actual)
                }
            }
        }
    }
}
```

Para ejecutar las pruebas unitarias, debe completar los pasos siguientes en la línea de comandos:

```azurecli
az login    # Required when no service principal environment variables are present
```

```shell
cd [Your GoPath]/src/staticwebpage
dep init    # Run only once for this folder
dep ensure  # Required to run if you imported new packages in test cases
cd test
go fmt
go test -run TestUT_StorageAccountName
```

El resultado de la prueba de Go tradicional se devuelve en aproximadamente un minuto.

### <a name="integration-test"></a>Prueba de integración

A diferencia de las pruebas unitarias, las pruebas de integración deben aprovisionar recursos en un entorno real para una perspectiva de un extremo a otro. Terratest hace un buen trabajo con este tipo de tareas. 

Entre los procedimientos recomendados para los módulos de Terraform se incluye la instalación de la carpeta `examples`. La carpeta `examples` contiene algunos ejemplos de un extremo a otro. Para evitar el trabajo con datos reales, ¿por qué no probar esos ejemplos como pruebas de integración? En esta sección, nos centramos en los tres archivos que están marcados con un asterisco `(*)` en la estructura de carpetas siguiente:

```
 📁 GoPath/src/staticwebpage
   ├ 📁 examples
   │   └ 📁 hello-world
   │       ├ 📄 index.html              (*)
   │       └ 📄 main.tf                 (*)
   ├ 📁 test
   │   ├ 📁 fixtures
   │   │   └ 📁 storage-account-name
   │   │       ├ 📄 empty.html
   │   │       └ 📄 main.tf
   │   ├ 📄 hello_world_example_test.go (*)
   │   └ 📄 storage_account_name_unit_test.go
   ├ 📄 main.tf
   ├ 📄 outputs.tf
   └ 📄 variables.tf
```

Comencemos con los ejemplos. Se crea una nueva carpeta de ejemplo llamada `hello-world/` en la carpeta `./examples/`. Aquí ofrecemos una página HTML sencilla para su carga: `./examples/hello-world/index.html`.

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Hello World</title>
</head>
<body>
    <h1>Hi, Terraform Module</h1>
    <p>This is a sample web page to demonstrate Terratest.</p>
</body>
</html>
```

El ejemplo de Terraform `./examples/hello-world/main.tf` es similar al que se muestra en la prueba unitaria. Hay una diferencia importante: el ejemplo también imprime la dirección URL del archivo HTML cargado como una página web llamada `homepage`.

```hcl
variable "website_name" {
  description = "The name of your static website."
  default     = "Hello-World"
}

module "staticwebpage" {
  source       = "../../"
  location     = "West US"
  website_name = var.website_name
}

output "homepage" {
  value = module.staticwebpage.homepage_url
}
```

Usamos de nuevo las funciones de prueba de Go clásicas y de Terratest en el archivo de prueba de integración `./test/hello_world_example_test.go`.

A diferencia de las pruebas unitarias, las pruebas de integración crean recursos reales en Azure. Por este motivo debe tener cuidado para evitar conflictos de nomenclatura. (Preste atención especial a algunos nombres únicos globales, como los nombres de cuenta de almacenamiento). Por lo tanto, el primer paso de la lógica de pruebas consiste en generar un `websiteName` aleatorio mediante el uso de la función `UniqueId()` que proporciona Terratest. Esta función genera un nombre aleatorio que tiene letras minúsculas, letras mayúsculas o números. `tfOptions` realiza todos los comandos de Terraform que tienen como destino la carpeta `./examples/hello-world/`. También se asegura de que `website_name` se establece en el `websiteName` aleatorio.

A continuación, se ejecutan `terraform init`, `terraform apply` y `terraform output`, uno por uno. Usamos otra función auxiliar, `HttpGetWithCustomValidation()`, que Terratest proporciona. Usamos la función auxiliar para asegurarnos de que el archivo HTML se carga en la dirección URL `homepage` de salida devuelta por `terraform output`. Comparamos el código de estado de HTTP GET con `200` y buscamos algunas palabras clave en el contenido HTML. Por último, se espera que `terraform destroy` se ejecute al aprovechar la característica de Go `defer`.

```go
package test

import (
    "fmt"
    "strings"
    "testing"

    "github.com/gruntwork-io/terratest/modules/http-helper"
    "github.com/gruntwork-io/terratest/modules/random"
    "github.com/gruntwork-io/terratest/modules/terraform"
)

func TestIT_HelloWorldExample(t *testing.T) {
    t.Parallel()

    // Generate a random website name to prevent a naming conflict
    uniqueID := random.UniqueId()
    websiteName := fmt.Sprintf("Hello-World-%s", uniqueID)

    // Specify the test case folder and "-var" options
    tfOptions := &terraform.Options{
        TerraformDir: "../examples/hello-world",
        Vars: map[string]interface{}{
            "website_name": websiteName,
        },
    }

    // Terraform init, apply, output, and destroy
    defer terraform.Destroy(t, tfOptions)
    terraform.InitAndApply(t, tfOptions)
    homepage := terraform.Output(t, tfOptions, "homepage")

    // Validate the provisioned webpage
    http_helper.HttpGetWithCustomValidation(t, homepage, func(status int, content string) bool {
        return status == 200 &&
            strings.Contains(content, "Hi, Terraform Module") &&
            strings.Contains(content, "This is a sample web page to demonstrate Terratest.")
    })
}
```

Para ejecutar las pruebas de integración, debe completar los pasos siguientes en la línea de comandos:

```azurecli
az login    # Required when no service principal environment variables are present
```

```shell
cd [Your GoPath]/src/staticwebpage
dep init    # Run only once for this folder
dep ensure  # Required to run if you imported new packages in test cases
cd test
go fmt
go test -run TestIT_HelloWorldExample
```

El resultado de la prueba de Go tradicional se devuelve en aproximadamente dos minutos. También podría ejecutar las pruebas unitarias y las pruebas de integración mediante la ejecución de estos comandos:

```shell
go fmt
go test
```

Las pruebas de integración tardan mucho más que las pruebas unitarias (dos minutos para un caso de integración comparado con un minuto para cinco casos unitarios). Pero es su decisión usar pruebas unitarias o pruebas de integración en un escenario. Normalmente, es preferible utilizar pruebas unitarias para una lógica compleja mediante las funciones HCL de Terraform. Normalmente usamos las pruebas de integración para la perspectiva de un extremo a otro de un usuario.

## <a name="use-mage-to-simplify-running-terratest-cases"></a>Uso de mage para simplificar la ejecución de los casos Terratest 
La ejecución de casos de prueba en Azure Cloud Shell requiere la ejecución de comandos diferentes en varios directorios. Para que este proceso sea más eficaz, incorporamos el sistema de compilación a nuestro proyecto. En esta sección usamos un sistema de compilación de Go, mage, para realizar el trabajo.

Lo único que necesita mage es el archivo `magefile.go` en el directorio raíz del proyecto (marcado con `(+)` en el ejemplo siguiente):

```
 📁 GoPath/src/staticwebpage
   ├ 📁 examples
   │   └ 📁 hello-world
   │       ├ 📄 index.html
   │       └ 📄 main.tf
   ├ 📁 test
   │   ├ 📁 fixtures
   │   │   └ 📁 storage-account-name
   │   │       ├ 📄 empty.html
   │   │       └ 📄 main.tf
   │   ├ 📄 hello_world_example_test.go
   │   └ 📄 storage_account_name_unit_test.go
   ├ 📄 magefile.go (+)
   ├ 📄 main.tf
   ├ 📄 outputs.tf
   └ 📄 variables.tf
```

Este es un ejemplo de archivo `./magefile.go`. En este script de compilación, escrito en Go, implementamos cinco pasos de compilación:
- `Clean`: este paso elimina todos los archivos generados y temporales que se generan durante las ejecuciones de la prueba.
- `Format`: este paso ejecuta `terraform fmt` y `go fmt` para dar formato a la base de código.
- `Unit`: este paso ejecuta todas las pruebas unitarias (con la convención de nombre de función `TestUT_*`) en la carpeta `./test/`.
- `Integration`: este paso es similar a `Unit`, pero en lugar de las pruebas unitarias, ejecuta las pruebas de integración (`TestIT_*`).
- `Full`: este paso ejecuta `Clean`, `Format`, `Unit` e `Integration` en secuencia.

```go
// +build mage

// Build a script to format and run tests of a Terraform module project
package main

import (
    "fmt"
    "os"
    "path/filepath"

    "github.com/magefile/mage/mg"
    "github.com/magefile/mage/sh"
)

// The default target when the command executes `mage` in Cloud Shell
var Default = Full

// A build step that runs Clean, Format, Unit and Integration in sequence
func Full() {
    mg.Deps(Unit)
    mg.Deps(Integration)
}

// A build step that runs unit tests
func Unit() error {
    mg.Deps(Clean)
    mg.Deps(Format)
    fmt.Println("Running unit tests...")
    return sh.RunV("go", "test", "./test/", "-run", "TestUT_", "-v")
}

// A build step that runs integration tests
func Integration() error {
    mg.Deps(Clean)
    mg.Deps(Format)
    fmt.Println("Running integration tests...")
    return sh.RunV("go", "test", "./test/", "-run", "TestIT_", "-v")
}

// A build step that formats both Terraform code and Go code
func Format() error {
    fmt.Println("Formatting...")
    if err := sh.RunV("terraform", "fmt", "."); err != nil {
        return err
    }
    return sh.RunV("go", "fmt", "./test/")
}

// A build step that removes temporary build and test files
func Clean() error {
    fmt.Println("Cleaning...")
    return filepath.Walk(".", func(path string, info os.FileInfo, err error) error {
        if err != nil {
            return err
        }
        if info.IsDir() && info.Name() == "vendor" {
            return filepath.SkipDir
        }
        if info.IsDir() && info.Name() == ".terraform" {
            os.RemoveAll(path)
            fmt.Printf("Removed \"%v\"\n", path)
            return filepath.SkipDir
        }
        if !info.IsDir() && (info.Name() == "terraform.tfstate" ||
            info.Name() == "terraform.tfplan" ||
            info.Name() == "terraform.tfstate.backup") {
            os.Remove(path)
            fmt.Printf("Removed \"%v\"\n", path)
        }
        return nil
    })
}
```

Puede usar los siguientes comandos para ejecutar un conjunto de pruebas completo. El código es similar a los pasos de ejecución que se usaron en una sección anterior. 

```azurecli
az login    # Required when no service principal environment variables are present
```

```shell
cd [Your GoPath]/src/staticwebpage
dep init    # Run only once for this folder
dep ensure  # Required to run if you imported new packages in magefile or test cases
go fmt      # Only required when you change the magefile
mage
```

Puede reemplazar la última línea de comandos por pasos de mage adicionales. Por ejemplo, puede usar `mage unit` o `mage clean`. Es una buena idea insertar comandos `dep` y `az login` en el archivo de mage. No mostramos aquí el código. 

Con mage, también puede compartir los pasos mediante el sistema de paquetes de Go. En ese caso, puede simplificar los archivos de mage en todos los módulos; para ello, haga referencia a una implementación común y declare las dependencias (`mg.Deps()`).

**Opcional: establecimiento de las variables de entorno de la entidad de servicio para ejecutar las pruebas de aceptación**
 
En lugar de ejecutar `az login` antes de las pruebas, puede realizar la autenticación de Azure estableciendo las variables de entorno de la entidad de servicio. Terraform publica una [lista de nombres de variables de entorno](https://www.terraform.io/docs/providers/azurerm/index.html#testing). (Solo las primeras cuatro de estas variables de entorno son obligatorias). Terraform también publica instrucciones detalladas que explican cómo [obtener el valor de estas variables de entorno](https://www.terraform.io/docs/providers/azurerm/authenticating_via_service_principal.html).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"] 
> [Página en GitHub de Terratest](https://github.com/gruntwork-io/terratest).