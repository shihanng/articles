---
title: tfvar - A tool to help you write Terraform's variable definitions
published: true
description: I built tfvar to improve my Terraforming workflow, hope this will help you too
tags: productivity, opensource, terraform, showdev
---

Whenever I want to start working on an existing [Terraform](https://www.terraform.io/) project or using a Terraform [module](https://www.terraform.io/docs/configuration/modules.html), the first thing that I find myself doing often is looking for the input variables.  If the project is small enough, the input variables are usually being declared in the `main.tf` file.  For a well organized Terraform configurations we can find a `variables.tf` file.  However, the locations or where to put the input variable declarations is just a convention and is not enforced syntactically.  It is our job as the user of the configurations/modules to track down all variables and make sure that they have the values [assigned properly](https://www.terraform.io/docs/configuration/variables.html#assigning-values-to-root-module-variables) before running `terraform plan` or `terraform apply`.  It is possible that some variables are hiding somewhere unexpectedly, or we just missed one during the assignment and only realize it when we encounter the following message during the `terraform plan` phase:

```terraform
var.bucket_name
  Enter a value:
```

**tfvar** is created to solve the problems mentioned above.  It is a CLI tool that extracts all variables declared in your Terraform configuration and output them in the format of .tfvars file so that you can use it as a template to write the variable definitions.

In this article, I would like to show you how this tool works.

## Simple Demo

Let's say we have the following variable declared in our Terraform configuration

```terraform
variable bucket_name {
  type    = string
  default = "mybucket"
}

variable instance_name {
  type = string
}
```

running `tfvar .` in the root directory of the Terraform configuration will give us the following in the standard output (stdout):

```sh
$ tfvar .
bucket_name   = "mybucket"
instance_name = null
```

Notice that the default value are automatically assigned in the generated definitions.  If we want to ignore the default values, there is a `--ignore-default` flag that we can use.  As shown below the with the `--ignore-default` flag, we get `null` instead:

```sh
$ tfvar . --ignore-default
bucket_name   = null
instance_name = null
```

A more common usage of **tfvar** might be to redirect the stdout to a [file](https://www.terraform.io/docs/configuration/variables.html#variable-definitions-tfvars-files) e.g. `inputs.tfvars` then change the value `null` in the file to a desired value.

```sh
$ tfvar . --ignore-default > inputs.tfvars
```

Terraform also allows input variables to be assigned [via environment variables](https://www.terraform.io/docs/configuration/variables.html#environment-variables).  For this, **tfvar** also provides the `-e` flag to generate a template in the environment variables format.

```sh
$ tfvar . --ignore-default -e
export TF_VAR_bucket_name=''
export TF_VAR_instance_name=''
```

If we already have some variables defined in `terraform.tfvars[.json]`, `*.auto.tfvars[.json]`, or environment variables (`TF_VAR_` followed by the name of a declared variable), (see section ["Assigning Values to Root Module Variables"](https://www.terraform.io/docs/configuration/variables.html#assigning-values-to-root-module-variables) in Terraform's documentation) and want to include there defined values in the generated one, we can use the `--auto-assign` flag:

```sh
$ export TF_VAR_bucket_name="mybucket"
$ export TF_VAR_instance_name="myinstance"
$ tfvar . --auto-assign
bucket_name   = "mybucket"
instance_name = "myinstance"
```

Finally for completeness sake, we also provide `--var` and `--var-file` flags to allow individual variables to be set, just like the `-var` and `-var-file` that we have in the `terraform (plan|apply)` commands.  However, I honestly could not think of any use case that I would need this in my workflow :laughing:.

All `--auto-assign`, `--var`, and `--var-file` flags can be used together, and when they are, they follow the variable definition precedence set by [Terraform](https://www.terraform.io/docs/configuration/variables.html#variable-definition-precedence).

## How to get it?

 The source codes of **tfvar** is publicly available on GitHub. In the repository below you will find the [installation instructions](https://github.com/shihanng/tfvar#installation).

{% github shihanng/tfvar %}

---

**tfvar** is a tool to solve a trivial problem that would only cost me a few minutes.  However, I decided to spend hours to build it hoping that it will save a few extra minutes off from my fellow Terraformers.  Some of the codes/ideas are borrowed from [Terraform itself](https://github.com/hashicorp/terraform) which I enjoy exploring a lot.  Please give it a try and I would love hear your feedback (via issues/PRs in project repository or comments in this post). If you like it, give the repository a :star: which will help other people discover it.

Thanks for reading :heart:.
