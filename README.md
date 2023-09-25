# Terraform Beginner Bootcamp 2023

## Semantic Versioning :mage:

This project is going to utilize semantic versioning for its tagging.
[semver.org](https://semver.org/)


The general format:

**MAJOR.MINOR.PATCH**, eg. `1.0.1`

- **MAJOR** MAJOR version when you make incompatible API changes
- **MINOR** MINOR version when you add functionality in a backward compatible manner
- **PATCH** PATCH version when you make backward compatible bug fixes

## Install the Terraform CLI
The Terraform CLI installation instructions have changed due to gpg keyring changes. So we needed refer to the latest install CLI instructions via Terraform Documentation and change the scripting for install 

[Install Terraform CLI](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

### Considerations for Linux Distribution

This project is built against Ubunutu.
Please consider checking your Linux Distribution and change accordingly to distribution needs.

[How to check os version in Linux](https://www.cyberciti.biz/faq/how-to-check-os-version-in-linux-command-line/)
Example of checking OS version:

```sh
$ cat /etc/os-release

PRETTY_NAME="Ubuntu 22.04.3 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.3 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy
```
### Refactoring into Bash Scripts
while fixing the terraform CLI gpg depreciaction issues we notice that bash scripts steps were a considerable amount more code. So we decided tp creat a bash script to install the terraform CLI.

This bash script is located here: [./bin/install_terraform_cli](/bin/install_terraform_cli.sh)]
- This will keep the gitpod Task File ([.gitpod.yml](.gitpod.yml)) tidy.
- This allow us an easier to debug and exeute manaully Terraform CLI install 
- This will allow better portability for other projects that need to install Terraform CLI

#### Shebang considerations

A shebang (prounced sha-bang) tells the bash script what program that will interpet the script. `#!/bin/bash`

ChatGPT recommended this format for bash `#!/usr/bin/env bash`

- For portability for different OS 
- Will search the user's Path for the bash executable 

https://en.wikipedia.org/wiki/Shebang_(Unix)

#### Execution considerations 

When executing the bash script we can use the `./` shorthand notiation to execute the bash script.

eg. `./bin/install_terraform_cli`

if we are using a script in .gitpod.yml we need to point the script to a profram to interpert it.

eg. `source ./bin/install_terraform_cli`

#### Linux Permissions Consideration 

In order to make our bash scripts executable we need to change linux permission to the executable at the user mode. 
```sh
chmod u+x ./bin/install_terraform_cli
```
Alternatively 

```sh
chmod 744 ./bin/install_terraform_cli
```
https://en.wikipedia.org/wiki/Chmod

### Git Lifecycle (before, init, Command)

We need to be careful when using the Init because it will not return if we restart an existing workspace.

https://www.gitpod.io/docs/configure/workspaces/tasks


### Working Env Vars 


#### Env Command 

We can list out all Environment Variables (Env Vars) using `env` command 

We can filter specific env vars using grep eg. `env | grep AWS_`

#### Setting and Unsetting Env Vars

In the terminal we can set using `export HELLO=``world`

In the terminal we unset using `unset HELLO`

We can set an env var temporarily when just running a command
```sh
HELLO=`world` ./bin/print_message
```
within a bash script we set env without writing export eg.

```sh
#!/usr/bin/env bash

HELLO=`world`

echo $HELLO
```

## Printing Vars

We can print an env var using echo eg. `echo $HELLO`

#### Scoping of Env Vars

when you open new bash terminals in VSCode it will not be aware of env vars that you have set in another window. 

if you want to Env Vars to persist across all future bash terminals that you are open ypu need to set env vars in your bash profile. eg. `.bash_profile`

#### Presisting Env Vars in Gitpod

We can persist env vars into gitpod by storing them in Gitpod secrets storage. 
```sh
gp env HELLOW=`world`
``` 
All future workspcaes lunched will set the env vars for all bash terminal opened in those workspaces.

You can also set env vars in the `.gitpod.yml` but this can only contain non-sensitive env vars.

### AWS CLI Installation

AWS CLI installed for the project via the bash script [`./bin/instakk_aws_cli`](./bin/install_aws_cli)

[Getting started Install (AWS CLI)](https://docs.
aws.amazon.com/cli/latest/userguide/getting-started-install.html)

We can check if our AWS creswntials is configured correctly by running the following AWS CLl command 

```sh
aws sts get-caller-identity'
```

if it succesful you see json payload return that looks like this:

```json
{
 "UserId": "AIDAXI32RULOLOLOLOLOD",
    "Account": "88882120101",
    "Arn": "arn:aws:iam::818105557086:user/terraform9999"
}
```

    We'll need to generate AWS CLI credits from IAM User in order to the user AWS CLI

## Terraform Basics 

### Terraform Registrey 

Terraform sources their providers and modules from terraform which is loacted  at [registry.terraform.io](https://registry.terraform.io/)

- **providers** is an interface to API that will allow to create resources in terraform.
- **modules** are a way to make to  make large amount of terraform code modular,portable and sharable. 

[Random Terraform Proviedr](https://registry.terraform.io/providers/hashicorp/random/latest/docs/resources/string)

### Terraform Console 

We can see a list of all the terraform commands by simply `terraform`

#### Terraform Init 

AT the start of a new terraform project we will run `terraform init` tp download the binaries for the terraform providers that we'll use in this project

#### Terraform Plan 

Creates an execution plan, which lets you preview the changes that Terraform plans to make to your infrastructure.

We can output this changeset ie. "plan" to be passed to an "Terrrafrom apply" but often you can just ignore outputting.

#### Terraform Apply 

 Executes the actions proposed in a terraform plan.

 If we want to automatically approve an apply we can provide the auto approve flag eg. `terraform apply --auto-approve`

 #### Terraform Destroy

 This will destroy resources.
 
 You can also use the auto approve flag to skip the approve prompt
 eg. `terraform apply --auto-approve`
 
 #### Terraform Lock File

 `/terraform.lock.file.hcl` contains the locked versioning for the providers or modulues that should be used with this project.

 The terraform Lock File **should be committed** to your Version Control system (VSC) eg. Gitub

### Terraform State Files

 `.terraform.tfstate` contain infromation about the current state of your infrastructure. 

 This file **should not be committed** tp your version contorl system (VSC) 

This file can contain sensitive data. 

if you lose this file, you lose knowing the state of your infrastructure.

`.terraform.tfstate.bacjup` is the previous state file state. 


## Issue with Terraform Cloud Login and Gitpod Workspace

When attempting to run terraform login it would launch bash a wiswig view to generate a token. However it does not work expexted in Gitpod  Vscode in the browser.

The workaround is manually generate a token in Terraform Cloud 

```
https://developer.hashicorp.com/terraform/tutorials/cloud-get-started/cloud-login
```

Once created then you run the command `terraform login`

The **output** should be like this:

```
Terraform will request an API token for app.terraform.io using your browser.

If login is successful, Terraform will store the token in plain text in
the following file for use by subsequent commands:
    /home/gitpod/.terraform.d/credentials.tfrc.json

Do you want to proceed?
  Only 'yes' will be accepted to confirm.

  Enter a value: 

```
**Enter yes**

The **output** will look like this:

```
Commands: Use arrow keys to move, '?' for help, 'q' to quit, '<-' to go back.
```

**Enter q**

**The output**
```
Are you sure you want to quit? (y) 
```

**Enter y**

**The output**
```
---------------------------------------------------------------------------------

Terraform must now open a web browser to the tokens page for app.terraform.io.

If a browser does not open this automatically, open the following URL to proceed:
    https://app.terraform.io/app/settings/tokens?source=terraform-login


---------------------------------------------------------------------------------

Generate a token using your browser, and copy-paste it into this prompt.

Terraform will store the token in plain text in the following file
for use by subsequent commands:
    /home/gitpod/.terraform.d/credentials.tfrc.json

Token for app.terraform.io:
  Enter a value: 
```
CTRL+C copy the token from Terraform cloud and CTRL+V yoyur token in **Enter a value** and hit enter

***Important note*** You Will not be able to see what you have pasted sicne you typing confidential, or secure type values.

You'll see the message:

``` 
You should successfully have entered your token, 
```