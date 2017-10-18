# Creating Azure Resources with Terraform

### Introduction

There are multiple ways to provision infrastructure into Azure today, from creating resources through the user interface in [Azure portal](http://portal.azure.com) to using Azure CLI to authoring Azure Resource Manager JSON-based templates. The last option - writing JSON-based definitions of infrastructure resources - gets us very close to fulfilling the Infrastructure-as-Code promise, but JSON syntax can be hard to read with double quotation marks and unintuitive comment placement.

This is where HashiCorp Terraform comes in, providing a way to deploy cloud infrastructure using a higher-level templating language. But beyond improved readability, Terraform templates allow you to use the same language for a variety of cloud providers, making it a valuable tool in any multi-cloud strategy. In this article, I will show you how to get started using Terraform with Azure. I will use an example of provisioning a virtual machine running Ubuntu OS in a resource group together with all the supporting cloud infrastructure (virtual network, public IP, etc.) necessary for that VM to run.


### Installing Terraform
The install process for Terraform is straightforward - [download](https://www.terraform.io/downloads.html) the package appropriate for your OS and unzip it into a separate install directory. The package contains a single executable file, which you should also define a global `PATH` for. Instructions on setting the PATH on Linux and Mac can be found on [this page](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux), while [this page](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows) contains instructions for setting the PATH on Windows. Verify your installation by running the `terraform` command - you should see a list of available Terraform options as output.

Let's allow Terraform to access your Azure subscription and perform provisioning steps next.


### Setting up Terraform Access to Azure
To unlock Terraform magic in Azure, you need to allow Terraform scripts to provision resources into your Azure subscriptions on your behalf. To enable that access, you need to setup two entities in Azure Active Directory (AAD) - AAD Application and AAD Service Principal - and use these entities' identifiers in your Terraform scripts. The reason for having both entities makes perfect sense in multi-tenant environments, where Service Principal is a local instance of a global AAD App and having a Service Principal allows for granular local access control to global resources.

When working with Terraform, however, you will be using a single AAD Application and a single Service Principal to enable resource provisioning, but you still need to create both. To streamline this setup process, HashiCorp and Microsoft have written scripts that create all the necessary security infrastructure for you in Azure (AAD App and Service Principal), allowing you to simply execute these scripts and copy/paste the necessary information into your Terraform code.

The steps below outline what you need to do to create an AAD Application and Service Principal using the scripts supplied.


#### Windows Users
If you are using a Windows machine to write and execute your Terraform scripts, you need to (1) [install Azure PowerShell tools](https://azure.microsoft.com/en-us/documentation/articles/powershell-install-configure/#step-1-install) and (2) download and execute the [azure-setup.ps1 script](https://github.com/echuvyrov/terraform101/blob/master/azureSetup.ps1) from the PowerShell console. To run the azure-setup.ps1 script, download it, execute the "./azure-setup.ps1 setup" command from the PowerShell console and login into your Azure subscription with administrative privileges. Then, provide an application name (arbitrary string, required) when prompted and (optionally) supply a strong password. If you don't provide password, the strong password will be generated for you using .Net security libraries.

#### Linux/Mac Users
To get started with Terraform on Linux machines or Macs, you need to (1) [install Azure xPlat CLI tools](https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-install/), (2) [download and install jq](https://stedolan.github.io/jq/download/) JSON processor and  (3) download and execute the [azure-setup.sh script](https://github.com/mitchellh/packer/blob/master/contrib/azure-setup.sh) bash script from the console. To run the azure-setup.sh script, download it, execute the "./azure-setup setup" command from the console and login into your Azure subscription with administrative privileges. Then, provide an application name (arbitrary string, required) when prompted and (optionally) supply a strong password when prompted. If you don't provide the password, the strong password will be generated for you using .Net security libraries.

Both Linux and Windows scripts create an AAD Application and a Service Principal, giving Service Principal an owner-level access on the subscription. Because of high level of access granted, you should always protect the security information generated by those scripts. Take a note of all four pieces of security information provided by those scripts: _client_id, client_secret, subscription_id and tenant_id_. Finally, if for some reason you were not able to execute the scripts, you can create a Service Principal manually by following this [step-by-step documentation](https://www.terraform.io/docs/providers/azurerm/index.html) from HashiCorp.
