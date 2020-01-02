
<!-- theme: spkane -->
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

<!-- footer: Copyright © 2015-2020, Sean P. Kane (@spkane) -->
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

# Providers (1 of 2)

* Providers
  * Individual plugins that enable terraform to properly interact with an API.
  * These can range between Hashicorp's officially supported providers to custom providers written by a single developer.

---

# Providers (2 of 2)

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

![bg contain](images/graph.png)

---

# The Setup

* `terraform init`

---

# The Plan

* `terraform plan`

---

# The Apply

* `terraform apply`
  * If all looks good, answer: `yes`

---

# The Outputs

* `terraform output`

---

# The State File

* `terraform12 state list`
* `terraform12 state show aws_instance.todo[0]`
* `terraform12 state pull > $HOME/class-terraform-starting/state.json`
* `less $HOME/class-terraform-starting/state.json`
* `rm $HOME/class-terraform-starting/state.json`

---

# Examine the Server

* `ssh -i $HOME/.ssh/oreilly_aws ubuntu@${todo_ip}`
* `sudo systemctl status todo-list`
* `exit`
* `cd ..`

---

# Test the Todo API

* `source ./bin/ip_vars.sh`
* `curl -i http://todo-api.spkane.org:8080/`
*  `curl -i http://todo-api.spkane.org:8080/ -X POST -H 'Content-Type: application/spkane.todo-list.v1+json' -d '{"description":"go shopping","completed":false}'`
* `curl -i http://todo-api.spkane.org:8080/`
* `curl -i http://todo-api.spkane.org:8080/1 -X DELETE -H 'Content-Type: application/spkane.todo-list.v1+json'`
* `curl -i http://todo-api.spkane.org:8080/`

---

# Download the Todo Provider

* Open in your web browser:
  * https://github.com/spkane/todo-for-terraform/releases/tag/v1.0.0
* Download the `terraform-provider-todo` archive for your platform.

---

# Install the Todo Provider

* `cd $HOME/Downloads` # or where ever you downloaded the archive to.
* `unzip terraform-provider-todo-*.zip`
* `mv terraform-provider-todo $HOME/class-terraform-starting/todo-for-terraform/terraform-tests/`

---

# Copy the Code

* `cd $HOME/class-terraform-starting/todo-for-terraform`
* `mkdir -p tf-code`
* `cp -a terraform-tests tf-code`
* `cd ./tf-code/terraform-tests`

---

# Defining Variables

* Open `variables.tf`
  * This is where we define variables we will use in the terraform code.

---

# Using the Todo Provider

* Open `main.tf`
  * Configure the todo provider
    * Change `host = "127.0.0.1"` to `host = "todo-api.spkane.org"`
  * Create 5 new todos
  * Read 1 existing todo as a data source
  * Create 5 more new todos based on the data source

---

# Defining Outputs

* Open `outputs.tf`
  * Prints the IDs for all of the new todos

---

# Prepare the Data

* We need a todo with ID 1 to read in as an example data source
* `curl -i http://todo-api.spkane.org:8080/`
* `curl -i http://todo-api.spkane.org:8080/ -X POST -H 'Content-Type: application/spkane.todo-list.v1+json' -d '{"description":"go shopping","completed":false}'`

---

# macOS Catalina+ Notice

* You may need to whitelist the provider binary, since it is not signed.
  * Run `./terraform-provider-todo`
    * Click `Cancel`
  * Go to `System Preferences` → `Security & Privacy` → `General`
    * Click `Allow Anyway`
  * Run `./terraform-provider-todo`
    * Click `Open`

---

# Apply Terraform Code

* `terraform init`
* `terrraform apply`
  * **Plan**: 10 to add, 0 to change, 0 to destroy.
    * If all looks good, answer: `yes`

---

# Examine the Outputs

* `terraform output`

* You may notice that your IDs are likely not in order. This is because, by default terraform creates many of the resources in parallel and we have many students using the server at the same time.

---

# Examine The State File

* Examine the state from one of the resulting todos
  * `state show todo.test1[0]`

  ```terraform
  # todo.test1[0]:
  resource "todo" "test1" {
      completed   = false
      description = "0-1 test todo"
      id          = "6"
  }
  ```

---

# The Real Object

* From the output of the last command, grab the ID and use it at the end of this command.
* `curl -i http://todo-api.spkane.org:8080/6`

```json
HTTP/1.1 200 OK
Date: Wed, 01 Jan 2020 20:13:45 GMT
Content-Type: application/spkane.todo-list.v1+json
Content-Length: 59
Connection: keep-alive

[{"completed":false,"description":"0-1 test todo","id":6}]
```

---

# Updating Objects

* Change the 2 `count = 5` lines to read `count = 4`
* Add `(updated)` to the end of the first description string.

---

# Code With Edits

```terraform
resource "todo" "test1" {
  count = 4
  description = "${count.index}-1 test todo (updated)"
  completed = false
}

resource "todo" "test2" {
  count = 4
  description = "${count.index}-2 test todo (linked to ${data.todo.foreign.id})"
  completed = false
}
```

---

# Examine The First & Last Todo

* `terraform state show todo.test1[0]`
* `terraform state show todo.test1[4]`

---

# Apply The Updates

* `terrraform apply`
  * **Plan**: 0 to add, 4 to change, 2 to destroy.
    * If all looks good, answer: `yes`

---

# Re-examine The First & Last Todo

* `terraform state show todo.test1[0]`
  * The description should now be updated.
* `terraform state show todo.test1[4]`
  * This should give you an error since it has now been deleted.
* `terraform state show todo.test1[3]` will work however, since we only have 4 todos now.

---

# Prepare to Import

* Create a new todo by hand
  * `curl -i http://todo-api.spkane.org:8080/ -X POST -H 'Content-Type: application/spkane.todo-list.v1+json' -d '{"description":"Imported Todo","completed":false}'`
  * Note the ID in your output (*13* in this example): `{"completed":false,"description":"Imported Todo","id":13}`

---

# Modify The Code

* In `main.tf` add:

```terraform
resource "todo" "imported" {
  description = "Imported Todo"
  completed = false
}
```

---

# Run a Plan

* `terraform plan`
  * You should see: **Plan**: 1 to add, 0 to change, 0 to destroy.

```terraform
  # todo.imported will be created
  + resource "todo" "imported" {
      + completed   = false
      + description = "Imported Todo"
      + id          = (known after apply)
    }
```

---

# Import a Pre-Existing Todo

* Import the ID of the Todo that you just created.
  * `terraform import todo.imported[0] 13`

---

# Re-run the Plan

* `terraform plan`
  * You should see
    * **No changes. Infrastructure is up-to-date.**

---

# Rename a Resource

* In `main.tf`:
  * change the line `resource "todo" "imported" {` to read `resource "todo" "primary" {`
* Run `terraform plan`
  * You should see
    * **Plan**: 1 to add, 0 to change, 1 to destroy.
* This would delete one todo and create a new one.
  * This is not what we want.

---

# Manipulating State

* `terraform state mv todo.imported todo.primary`
* Run `terraform plan`
  * You should see
    * **No changes. Infrastructure is up-to-date.**
* By moving the state of the existing resource to the new name, everything lines back up properly.

---

# Terraform Modules

* Blocks of re-useable Terraform code w/ inputs and outputs
* `cd ..`
* `cp -a ../__modules .`
* `cd __modules/todo-test-data`

---

# Module Variables

* Open `variables.tf`
  * This files defines all the variables that the module uses and any default values.

---

# The Main Module Code

* Open `main.tf`
  * This files will allow us to easily create two set of todos matching our specific requirements.

---

# Module Outputs

* Open `outputs.tf`
  * If you think of modules like functions then outputs are return values.

---

# Prepare to Use the Module

* `cd ../../code/`
* Open `main.tf`

---

# Utilize the Module (1 of 2)

* Add:

```terraform
module "series-data" {
    source       = "../__modules/todo-test-data"
    number       = 5
    purpose      = "testing"
    team_name    = "oreilly"
    descriptions = ["my first completed todo", "my second completed todo",
                    "my third completed todo", "my fourth completed todo",
                    "my fifth completed todo"
                   ]
}
```

---

# Utilize the Module (2 of 2)

* Open `outputs.tf`
* Add:

```terraform
output "first_series_ids" {
  value = "${module.series-data.first_series_ids}"
}

output "second_series_ids" {
  value = "${module.series-data.second_series_ids}"
}
```

---

# Apply the Module

* `terraform apply`
  * If all looks good, answer: `yes`

---

# Destroy the Todos

* `terraform destroy`
  * **Plan**: 0 to add, 0 to change, 9 to destroy.
    * If all looks good, answer: `yes`

---

# Destroy the Infrastructure

* `cd ../terraform-infrastructure/`
* `terraform destroy`
  * If all looks good, answer: `yes`

---

# What We Have Learned

* How to install Terraform
* The primary use case for Terraform
* How to install a provider and what they are for
* Creating, reading, updating, and deleting objects
* Reading data sources & importing existing objects
* Making & using modules
* What the Terraform state is
* and more...

---

<!-- _class: lead -->

# Additional Reading

* [Terraform: Up & Running](https://www.terraformupandrunning.com/)
* [Terraform Documentation](https://www.terraform.io/docs/index.html)

---

<!-- _class: lead -->

# Additional Learning Resources


## https://learning.oreilly.com/

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

