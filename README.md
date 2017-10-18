# Creating Azure Resources with Terraform

### Introduction

There are multiple ways to provision infrastructure into Azure today, from creating resources through the user interface in [Azure portal](http://portal.azure.com) to using Azure CLI to authoring Azure Resource Manager JSON-based templates. The last option - writing JSON-based definitions of infrastructure resources - gets us very close to fulfilling the Infrastructure-as-Code promise, but JSON syntax can be hard to read with double quotation marks and unintuitive comment placement.

This is where HashiCorp Terraform comes in, providing a way to deploy cloud infrastructure using a higher-level templating language. But beyond improved readability, Terraform templates allow you to use the same language for a variety of cloud providers, making it a valuable tool in any multi-cloud strategy. In this article, I will show you how to get started using Terraform with Azure. I will use an example of provisioning a virtual machine running Ubuntu OS in a resource group together with all the supporting cloud infrastructure (virtual network, public IP, etc.) necessary for that VM to run.


### Installing Terraform
The install process for Terraform is straightforward - [download](https://www.terraform.io/downloads.html) the package appropriate for your OS and unzip it into a separate install directory. The package contains a single executable file, which you should also define a global `PATH` for. Instructions on setting the PATH on Linux and Mac can be found on [this page](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux), while [this page](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows) contains instructions for setting the PATH on Windows. Verify your installation by running the `terraform` command - you should see a list of available Terraform options as output.

Let's allow Terraform to access your Azure subscription and perform provisioning steps next.
