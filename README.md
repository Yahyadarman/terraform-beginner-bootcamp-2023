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
