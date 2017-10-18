# Creating Azure Resources with Terraform

### Introduction

There are multiple ways to provision infrastructure into Azure today, from creating resources through the user interface in [Azure portal](http://portal.azure.com) to using Azure CLI to authoring Azure Resource Manager JSON-based templates. The last option - writing JSON-based definitions of infrastructure resources - gets us very close to fulfilling the Infrastructure-as-Code promise, but JSON syntax can be hard to read with double quotation marks and unintuitive comment placement.

This is where HashiCorp Terraform comes in, providing a way to deploy cloud infrastructure using a higher-level templating language. But beyond improved readability, Terraform templates allow you to use the same language for a variety of cloud providers, making it a valuable tool in any multi-cloud strategy. In this article, I will show you how to get started using Terraform with Azure. I will use an example of provisioning a virtual machine running Ubuntu OS in a resource group together with all the supporting cloud infrastructure (virtual network, public IP, etc.) necessary for that VM to run.
