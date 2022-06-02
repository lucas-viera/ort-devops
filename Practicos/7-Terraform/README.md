# Terraform

# Parte 01 - Terraform basics

# Terraform & AWS CLI Installation

## 01 - Introducción
- Instalar Terraform CLI
- Instalar AWS CLI
- Instalar VS Code Editor
- Instalar HashiCorp Terraform plugin for VS Code


## 02 -  macOS: Instalar Terraform
- [Download Terraform MAC](https://www.terraform.io/downloads.html)
- [Install CLI](https://learn.hashicorp.com/tutorials/terraform/install-cli)
- Descompromit el archivo
```
# Copy binary zip file to a folder
mkdir /Users/<YOUR-USER>/Documents/terraform-install
COPY Package to "terraform-install" folder

# Unzip
unzip <PACKAGE-NAME>
unzip terraform_0.14.3_darwin_amd64.zip

# Copy terraform binary to /usr/local/bin
echo $PATH
mv terraform /usr/local/bin

# Verify Version
terraform version

# To Uninstall Terraform (NOT REQUIRED)
rm -rf /usr/local/bin/terraform
``` 

## 03 - macOS: IDE para Terraform - VS Code Editor
- [Microsoft Visual Studio Code Editor](https://code.visualstudio.com/download)
- [Hashicorp Terraform Plugin for VS Code](https://marketplace.visualstudio.com/items?itemName=HashiCorp.terraform)


### 04 - macOS: Instalar AWS CLI
- [AWS CLI Install](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)
- [Install AWS CLI - MAC](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-mac.html#cliv2-mac-install-cmd)

```
# Install AWS CLI V2
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
sudo installer -pkg AWSCLIV2.pkg -target /
which aws
aws --version

# Uninstall AWS CLI V2 (NOT REQUIRED)
which aws
ls -l /usr/local/bin/aws
sudo rm /usr/local/bin/aws
sudo rm /usr/local/bin/aws_completer
sudo rm -rf /usr/local/aws-cli
```


## 05 - macOS: Configure AWS Credentials 
- **Pre-requisite:** Tener una cuenta de AWS.
  - [Create an AWS Account](https://portal.aws.amazon.com/billing/signup?nc2=h_ct&src=header_signup&redirect_url=https%3A%2F%2Faws.amazon.com%2Fregistration-confirmation#/start)
- Generate Security Credentials using AWS Management Console
  - Go to Services -> IAM -> Users -> "Your-Admin-User" -> Security Credentials -> Create Access Key
- Configure AWS credentials using SSH Terminal on your local desktop
```
# Configure AWS Credentials in command line
$ aws configure
AWS Access Key ID [None]: AKIASUF7DEFKSIAWMZ7K
AWS Secret Access Key [None]: WL9G9Tl8lGm7w9t7B3NEDny1+w3N/K5F3HWtdFH/
Default region name [None]: us-east-1
Default output format [None]: json

# Verify if we are able list S3 buckets
aws s3 ls
```
- Verify the AWS Credentials Profile
```
cat $HOME/.aws/credentials 
```

## 06 - Windows: Terraform & AWS CLI Install
- [Download Terraform](https://www.terraform.io/downloads.html)
- [Install CLI](https://learn.hashicorp.com/tutorials/terraform/install-cli)
- Descomprimir el archivo
- Crear directorio `terraform-bins`
- Copiar `terraform.exe` a `terraform-bins`
- Agregar PATH en windows (variable de ambiente)
- Instalar [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)

## 07 -  LinuxOS: Terraform & AWS CLI Install
- [Download Terraform](https://www.terraform.io/downloads.html)
- [Linux OS - Terraform Install](https://learn.hashicorp.com/tutorials/terraform/install-cli)

# Terraform Command Basics

## 01 - Introducción
- Entendiendo los comandos básicos de Terraform
  - terraform init
  - terraform validate
  - terraform plan
  - terraform apply
  - terraform destroy      

## 02 - Revisar terraform manifest para EC2 Instance
- **Pre-condición-1:** Verificar que se tiene **default-vpc** en la región del manifest.
- **Pre-condición-2:** Verificar que el AMI que se encuentra en el manifest existe en la región de manifest, en caso contrario, actualizar por un AMI existente en la región
- **Pre-condición-3:** Verificar las AWS Credentials en **$HOME/.aws/credentials**
```t
# Terraform Settings Block
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      #version = "~> 3.21" # Optional but recommended in production
    }
  }
}

# Provider Block
provider "aws" {
  profile = "default" # AWS Credentials Profile configured on your local desktop terminal  $HOME/.aws/credentials
  region  = "us-east-1"
}

# Resource Block
resource "aws_instance" "ec2demo" {
  ami           = "ami-04d29b6f966df1537" # Amazon Linux in us-east-1, update as per your region
  instance_type = "t2.micro"
}
```

## 03 - Terraform Core Commands
```t
# Initialize Terraform
terraform init

# Terraform Validate
terraform validate

# Terraform Plan to Verify what it is going to create / update / destroy
terraform plan

# Terraform Apply to Create EC2 Instance
terraform apply 
```

## 04 - Verificar la instancia EC2 Instance en AWS Management Console
- Ir a AWS Management Console -> Services -> EC2
- Verificar que la instancia EC2 fue creada.



## 05 - Destruir la Infraestructura
```t
# Destroy EC2 Instance
terraform destroy

# Delete Terraform files 
rm -rf .terraform*
rm -rf terraform.tfstate*
```

## 06 - Conclusión
- Resumen de lo que aprendimos durante esta lección.
- Aprendimos sobre los comandos más importantes de Terraform:
  - terraform init
  - terraform validate
  - terraform plan
  - terraform apply
  - terraform destroy     

# Terraform Configuration Language Syntax

## 01 - Introducción
- Entender Terraform Language Basics
  - Entender Blocks
  - Entender Arguments, Attributes & Meta-Arguments
  - Entender Identifiers
  - Entender Comments
 


## 02 - Terraform Configuration Language Syntax
- Entender Blocks
- Entender Arguments
- Entender Identifiers
- Entender Comments
- [Terraform Configuration](https://www.terraform.io/docs/configuration/index.html)
- [Terraform Configuration Syntax](https://www.terraform.io/docs/configuration/syntax.html)
```t
# Template
<BLOCK TYPE> "<BLOCK LABEL>" "<BLOCK LABEL>"   {
  # Block body
  <IDENTIFIER> = <EXPRESSION> # Argument
}

# AWS Example
resource "aws_instance" "ec2demo" { # BLOCK
  ami           = "ami-04d29b6f966df1537" # Argument
  instance_type = var.instance_type # Argument with value as expression (Variable value replaced from varibales.tf
}
```

## 03 -  Entender sobre Arguments, Attributes and Meta-Arguments.
- Los argumentos puede ser `required` o `optional`
- El formato de los atributos es `resource_type.resource_name.attribute_name`
- Meta-Arguments pueden cambiar comportamiento de los recursos (Ejemplo: count, for_each)
- [Additional Reference](https://learn.hashicorp.com/tutorials/terraform/resource?in=terraform/configuration-language) 
- [Resource: AWS Instance](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance)
- [Resource: AWS Instance Argument Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance#argument-reference)
- [Resource: AWS Instance Attribute Reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance#attributes-reference)
- [Resource: Meta-Arguments](https://www.terraform.io/docs/language/meta-arguments/depends_on.html)

## 04 - Entendiendo sobre Terraform Top-Level Blocks
- Informarse sobre Terraform Top-Level blocks
  - Terraform Settings Block
  - Provider Block
  - Resource Block
  - Input Variables Block
  - Output Values Block
  - Local Values Block
  - Data Sources Block
  - Modules Block





# Parte 02 - Fundamental Blocks

# Terraform Block 

## 01 - Introducción
- Entender sobre los Terraform Block y su importancia.
- Entender como manejar restricciones de versión para Terraform version y Provider version en Terraform Block.

## 02 - Entender sobre Terraform Settings Block
- Terraform Version requerida
- Provider Requirements
- Terraform backends
- Experimental Language Features
- Pasar Metadata to Providers
- Revisar el archivo **sample-terraform-settings.tf** para mayor entendimiento

## 03 - Crear un simple terraform block y jugar con el required_version
- `required_version` se centra en el Terraform CLI instalado en su equipo.
- Si la versión instalada en su escritorio no coincide con las restricciones especificadas en el Terraform block, se producirá un error.
- Cambie las versiones y ejecute `terraform init`, observe los comportamientos.
```
Play with Terraform Version
  required_version = "~> 0.14.3" 
  required_version = "= 0.14.3"    
  required_version = "= 0.14.4"  
  required_version = ">= 0.13"   
  required_version = "= 0.13"    
  required_version = "~> 0.13"   
 

# Terraform Block
terraform {
  required_version = "~> 0.14"
}

# To view my Terraform CLI Version installed on my desktop
terraform version

# Initialize Terraform
terraform init
```
## 04 - Agregar Provider y jugar con el Provider Version 
- `required_providers` especifica todos los proveedores requeridos por el módulo actual, asignando cada nombre de proveedor local a una dirección origen y una restricción de versión. 
- Realizar el mismo ejercicio que el punto anterior.

```
Play with Provider Version
      version = "~> 3.0"            
      version = ">= 3.0.0, < 3.1.0"
      version = ">= 3.0.0, <= 3.1.0"
      version = "~> 2.0"
      version = "~> 3.0"   
```

```
# Terraform Init with upgrade option to change provider version
terraform init -upgrade
```


## 05 - Clean-Up
```
# Delete Terraform Folders & Files
rm -rf .terraform*
```

## Referencias
- [Terraform Version Constraints](https://www.terraform.io/docs/configuration/version-constraints.html)
- [Terraform Versions - Best Practices](https://www.terraform.io/docs/configuration/version-constraints.html#best-practices)

# Terraform Provider Block

## 01 - Introducción
- Qué son los Terraform Providers?
- Qué hacen los Providers?
- En donde estan alojados los Providers (Terraform Registry)?
- Para qué se usan los Providers?


## 02 - Provider Requirements
- Definir los providers en Terraform Block
- Entender que significa cada uno de los siguientes terminos:
`required_providers` in terraform block
  - local names
  - source
  - version
```t
# Terraform Block
terraform {
  required_version = "~> 0.14.4"
  required_providers {
    aws = { 
      source = "hashicorp/aws"
      version = "~> 3.0"
    }
  }
}
```


## 03 - Provider Block  
- Crear un Provider Block for AWS
```t
# Provider Block
provider "aws" {
  region = "us-east-1"
  profile = "default"  # defining it is optional for default profile
}
```
- Verificar los tipos de [Authentication Types](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#authentication) 
- Static Credentials - NO RECOMENDADA
- Environment variables
- Credanciales compartidas/configuration file (Vamos a utilizar este último)
  - Verificar en `cat $HOME/.aws/credentials`
  - Si no lo encuentran, usar `aws configure` para configurar las credenciales de aws.

```t
# Initialize Terraform
terraform init

# Validate Terraform Configuration files
terraform validate

# Execute Terraform Plan
terraform plan
```  

## 04 - Agrear un Reosource Block para crear una AWS VPC
- [AWS VPC Resource](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/vpc)
```t
resource "aws_vpc" "myvpc" {
  cidr_block = "10.0.0.0/16"
  tags = {
    "Name" = "myvpc"
  }
}
```

## 05 - Ejecutar los comandos de Terraform para crear una AWS VPC
```t
# Initialize Terraform
terraform init

# Validate Terraform Configuration files
terraform validate

# Execute Terraform Plan
terraform plan

# Create Resources using Terraform Apply
terraform apply -auto-approve
```  

## 06 - Clean-Up 
```t
# Destroy Terraform Resources
terraform destroy -auto-approve

# Delete Terraform Files
rm -rf .terraform*
rm -rf terraform.tfstate*
```


## Referencias
- [Terraform Providers](https://www.terraform.io/docs/configuration/providers.html)
- [AWS Provider Documentation](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [AWS VPC](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/vpc)


# Multiple Provider Configurations

## 01 - Introducción
- Entender y implementar configuración de multiples providers.

## 02 - ¿Cómo definir multiples configuraciones del mismo provider?  
- Entender sobre el provider por defecto.
- Entender y definir multiples prodivers del mismo tipo de provider.
```t
# Provider-1 for us-east-1 (Default Provider)
provider "aws" {
  region = "us-east-1"
  profile = "default"
}

# Provider-2 for us-west-1
provider "aws" {
  region = "us-west-1"
  profile = "default"
  alias = "aws-west-1"
}
```

## 03 - ¿Cómo referencias un provider que no sea el por defecto en un recurso?
```t
# Resource Block to Create VPC in us-west-1
resource "aws_vpc" "vpc-us-west-1" {
  cidr_block = "10.2.0.0/16"
  #<PROVIDER NAME>.<ALIAS>
  provider = aws.aws-west-1
  tags = {
    "Name" = "vpc-us-west-1"
  }
}
```

## 04 - Ejecutar los siguientes comandos
```t
# Initialize Terraform
terraform init

# Validate Terraform Configuration Files
terraform validate

# Generate Terraform Plan
terraform plan

# Create Resources
terraform apply -auto-approve

# Verify the same
1. Verify the VPC created in us-east-1
2. Verify the VPC created in us-west-2
```

## 05 - Clean-Up 
```t
# Destroy Terraform Resources
terraform destroy -auto-approve

# Delete Terraform Files
rm -rf .terraform*
rm -rf terraform.tfstate*
```



## Referencias
- [Provider Meta Argument](https://www.terraform.io/docs/configuration/meta-arguments/resource-provider.html)


# Providers - Dependency Lock File

## 01 - Introducción
- Entender la importancia de Dependency Lock File que aparece a partir de la versión de Terraforn 0.14.

## 02 - Revisar los Terraforn Manifests
- Ver que la información em los archivos de la carpeta terraform-manifesta y validar en que difieren de los ejercicios anteriores. 

- c1-versions.tf
- c2-s3bucket.tf
- .terraform.lock.hcl


## 03 - Inicializar y aplicar la configuración 
```t
# Initialize Terraform
terraform init

# Validate Terraform Configuration files
terraform validate

# Execute Terraform Plan
terraform plan

# Create Resources using Terraform Apply
terraform apply
```
- Verificar **.terraform.lock.hcl**
  - Ver el contenido del archivo
  - Comparar `.terraform.lock.hcl-ORIGINAL` & `.terraform.lock.hcl`
  - Hacer respaldo de `.terraform.lock.hcl` como `.terraform.lock.hcl-FIRST-INIT` 
```
# Backup
cp .terraform.lock.hcl .terraform.lock.hcl-FIRST-INIT
```

## 04 - Hacer version upgrade del AWS provider 
- Para AWS Provider, con la version `version = ">= 2.0.0"`, va a ser actualizado a la última versióni `3.x.x` con el comando `terraform init -upgrade` 
```t
# Upgrade Provider Version
terraform init -upgrade
```
- Revisar **.terraform.lock.hcl**
  - Discutir sobre las versiones de AWS.
  - Comparar `.terraform.lock.hcl-FIRST-INIT` & `.terraform.lock.hcl`

## 05 - Aplicar la configuración de Terraform con el último AWS Provider 
- Debería de fallar el despliegue por los cambios realizados sobre la versión de AWS.
```
# Terraform Apply
terraform apply
```

## 06 - Comentar la región en el recurso que fallo y volver a probar
- Cuando actualizamos a una versión mayor de provider, se pueden romper algunas funcionalidades.
- Por eso con `.terraform.lock.hcl`, podemos evitar este tipo de problemas.
```
# Comment Region Attribute
# Resource Block: Create AWS S3 Bucket
resource "aws_s3_bucket" "sample" {
  bucket = random_pet.petname.id
  acl    = "public-read"

  #region = "us-west-2"
}

# Terraform Apply
terraform apply
```

## 07 - Clean-Up
```
# Destroy Resources
terraform destroy

# Delete Terraform Files
rm -rf .terraform    # We are not removing files named ".terraform.lock.hcl, .terraform.lock.hcl-ORIGINAL" which are needed for this demo for you.
rm -rf terraform.tfstate*
```

## Referencias
- [Random Pet Provider](https://registry.terraform.io/providers/hashicorp/random/latest/docs/resources/pet)
- [Dependency Lock File](https://www.terraform.io/docs/configuration/dependency-lock.html)
- [Terraform New Features in v0.14](https://learn.hashicorp.com/tutorials/terraform/provider-versioning?in=terraform/0-14)
- [AWS S3 Bucket Region - Read Only in AWS Provider V3.x](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/guides/version-3-upgrade#region-attribute-is-now-read-only)



## Parte 03 - Terraform Resources





## Parte 04 - Variables
## Parte 05 - Datasources
## Parte 06 - State
## Parte 07 - Workspaces
## Parte 08 - Provisioners
## Parte 09 - Modules
## Parte 10 - Cloud And Enterprise Capabilities