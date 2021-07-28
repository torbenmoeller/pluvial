+++
author = "Torben"
title = "Easy setup of a development environment"
date = "2021-07-11"
description = "Using winget can automate your setup (partially)"
categories = ["cookbook",]
tags = ["winget",]
+++

# TL;DR
If you create a script to install all software and store it on an external medium like GitHub, 
you can automate the setup of a fresh OS. 

You can finally download a web browser without the Internet explorer 😋

# Motivation 
* Setup of a fresh OS can be tedious
* Setup of multiple PCs can be even more tedious
* Installers are distributed all over the internet and be hard to find
* Installers from untrusted sources could contain malware 
* Installers could be for old versions of the software
* Manual execution of installers needs attention of a person

# Prerequisites
Installed Winget. You can get winget from
* [Microsoft Store](ms-windows-store://pdp/?ProductId=9wzdncrfj364)
* [Manual Install](https://github.com/microsoft/winget-cli/releases/)
* [Preinstalled on Windows 10 development environment](https://developer.microsoft.com/en-us/windows/downloads/virtual-machines/)

# Approach
The Windows Package Manager (aka winget) is a package manager from Microsoft for Windows.
Similiar to apt or yum, winget helps with the process of installing, upgrading and removing applications.
After installation, winget offers a command line  
Create a script and add your software. 

**Common Tools**
{{< highlight bash >}}
# Common Tools
winget install --id=Notepad++.Notepad++ -e -h
winget install --id=Mozilla.Firefox -e -h
winget install --id=Google.Chrome -e -h

# Common Dev Tools
winget install --id=Git.Git -e -h
winget install --id=Microsoft.VisualStudioCode -e -h
winget install --id=Microsoft.WindowsTerminal -e -h
# Kubernetes
winget install --id=Kubernetes.minikube -e -h
winget install --id=Mirantis.Lens -e -h
{{< / highlight >}}

**Languages**
{{< highlight bash >}}
# Python
winget install --id=Python.Python.3 -e -h
winget install --id=JetBrains.PyCharm.Professional -e -h
# Java
winget install --id=AdoptOpenJDK.OpenJDK.11 -e -h
winget install --id=JetBrains.IntelliJIDEA.Ultimate -e -h
# Go
winget install --id=GoLang.Go -e -h
winget install --id=JetBrains.GoLand -e -h
{{< / highlight >}}

**Cloud Tools**
{{< highlight bash >}}
# AWS
winget install --id=Amazon.AWSCLI -e -h
winget install --id=Amazon.SAM-CLI -e -h -h
# Azure
winget install --id=Microsoft.AzureCLI -e -h
winget install --id=Microsoft.AzureFunctionsCoreTools -e -h
# GCP
winget install --id=Google.CloudSDK -e -h
{{< / highlight >}}


# Discussion
* Installing .zip files is [not yet supported](https://github.com/microsoft/winget-cli/issues/140), 
  so tools like terraform, helm, maven have to be installed manually.
* [Chocolatey](https://chocolatey.org/) can be used together with winget or as an alternative. 
  Chocolatey also supports installation of zip files. 

# Conclusion
If you create a script to install all software and store it on an external medium like GitHub,
you can automate the setup of a fresh OS.
