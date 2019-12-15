
<!-- $theme: spkane -->
<!-- _class: lead -->

<!-- $size: 16:9 -->

# Terraform
## Getting Started

---

<!-- _class: lead -->

# Instructor
# Sean P. Kane

### @spkane

![bg right](images/skane-2018-side-black-cropped.jpg)

---

<!-- footer: Copyright © 2015-2019, Sean P. Kane (@spkane) -->
<!-- paginate: true -->
<!-- _class: lead -->

# [Follow Along Guide](https://gist.github.com/spkane/df02f7858f6bef9f045fa1c296f8146f)
## Textual Slides

---

# Prerequisites
* A recent computer and OS
  * Recent Linux, OS X, or Windows 10
  * Reliable and fast internet connectivity
* Hashicorp Terraform

---

# Prerequisites
* A graphical web browser
* A text editor
* A software package manager
* Git client
* General comfort with the command line will be helpful.
* [optional] tar, wget, curl, jq, SSH client

---

# A Note for Windows Users
This class was written from a largely Unix based perspective, but everything can be made to work in Windows with very little effort.
* Unix Variables
  * `export MY_VAR=test`
  * `echo ${MY_VAR}`
* Windows 10 Variables (powershell)
  * `$env:my_var = "test"`
  * `Get-ChildItem Env:my_var`

---

# A Note About Proxies
Proxies can interfere with some activities if they are not configured correctly.

---

# Instructor Environment
* **Operating System**: Mac OS X (v10.15.X+)
* **Terminal**: iTerm2 (Build 3.X.X+) - https://www.iterm2.com/
* **Shell Customization**: Bash-it - https://github.com/Bash-it/bash-it
* **Shell Prompt Theme**: Starship - https://starship.rs/
* **Shell Prompt Font**: Fira Code - https://github.com/tonsky/FiraCode
* **Text Editor**: Visual Studio Code (v1.X.X+) - https://code.visualstudio.com/

---

# Definition

* **ter·ra·form**
* */ˈterəˌfôrm/*
  * (verb) to alter a planet for the purpose of sustaining life

---

# Hashicorp Terraform

* Terraform is a tool that makes it possible to document and automate the creation, modification, and destruction of almost anything that can be managed by an API.
* This means that it is finally conceivable to automate the management of everything that your software stacks needs to actually run in any environment, including cloud resources, DNS entries, CDN configuration, and much more.

---

# Installing Terraform

* Download:
  * https://www.terraform.io/downloads.html
* Unarchive and copy into your executable $PATH

---

# Version 0.12

* Version 0.12 of terraform was a major release that included many significant improvements, but also included some breaking changes.
* Be aware that code written for terraform 0.12 is not compatible with earlier releases and that in general, you should not use not use older terraform binaries with existing terraform managed infrastructure.

---

# Code Setup

```shell
$ cd ${HOME}
$ mkdir class-terraform-starting
$ cd ${HOME}/class-terraform-starting
$ git clone https://github.com/spkane/todo-for-terraform \
  --config core.autocrlf=input
$ cd todo-for-terraform
```

---

# Exploring Terraform

```shell
$ cd terraform-infrastructure
```

---

# HCL & JSON

* Hashicorp Configuration Language v2
  * https://github.com/hashicorp/hcl/tree/hcl2
* HCL is a JSON-compatible configuration language written by Hashicorp to be machine and human friendly.

* HCL is intended to provide a less-verbose JSON style configuration language that supports comments, while also providing humans with a language that is easier to approach than YAML.


---

# Terraform & Backends

* Open `main.tf`
  * Terraform block
    * Define high-level requirements for this associated HCL. Terraform and provider version, etc.
  * Backend block
    * Define where remote state is stored and any information required to read and write it.

---

# Providers

  * Providers
    * Individual plugins that enable terraform to properly interact with an API.
    * These can range between Hashicorp's officially supported providers to custom providers written by a single developer.
    * In this example we are using the `aws` and `ns1` providers.
      * https://github.com/terraform-providers/terraform-provider-aws
      * https://github.com/terraform-providers/terraform-provider-ns1

---

# Variables

* Open `variables.tf`
  * Defines all the variables that you will be using and their default values.
* You will get errors if you use variables that are not defined in this file.

---

# Data Sources

* Open `data.tf`
* Using output as input
  * Remote Terraform State
  * APIs
  * Scripts
    * Open `bin/local-ip.sh`
  * etc

---

# Building Infrastructure

* `key-pairs.tf`
* `backend.tf`
* `frontend.tf`
* `security-groups.tf`

---

# Backend Service

* Open `key-pairs.tf`
  * SSH public key for system access
* Open `backend.tf`
  * Server Instance w/ basic provisioning
  * Setup of `todo` backend service
* The files in `./files` support the system provisioning.

---

# Frontend Infrastructure

* Open `frontend.tf`
  * S3 bucket (file share) for Load Balancer Logs
    * Security Policy for access to S3 bucket
  * Load Balancer for backend `todo` service
    * Listener
    * Target Group
    * Target Group Attachment
  * DNS record for load balancer

---

# Firewall Security

* Open `security-groups.tf`
  * SSH to the backend server
  * Traffic between load balancer and `todo` service

---

# Outputs

* Open `outputs.tf`
  * Human and computer-readable data

---

# The Graph

![bg](images/graph.png)

---

# What We Have Learned

* Something

---

# Additional Reading

* Something
---

<!-- _class: lead -->

# Additional Learning Resources


## https://www.safaribooksonline.com/

---

<!-- _class: lead -->

# Student Survey

**Please take a moment to fill out the class survey linked to in the chat channel.**

O’Reilly and I value your comments about the class.

Thank you!

---

<!-- _class: lead -->

# Any Questions?

---

