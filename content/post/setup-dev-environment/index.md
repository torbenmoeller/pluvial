+++
author = "Torben"
title = "Jump start your windows setup"
date = "2021-08-03"
description = "Or: Install Chrome without using the Internet Explorer"
categories = ["Cookbook",]
tags = ["Developer Experience", "Winget", "Windows", "Cmd", "Powershell",]
+++

# TL;DR
Winget is a new tool published by Microsoft.
It helps with software installation and management.
Winget can be used in scripts automatically install software on Windows. 

And: You can finally download a web browser without the Internet Explorer or Edge 😋

# Motivation 
* Setup of a fresh OS can be tedious
* Setup of multiple PCs can be even more tedious
* Installers are distributed all over the internet and be hard to find
* Installers from untrusted sources could contain malware 
* Installers could be for old versions of the software
* Manual execution of installers needs attention of a person

# Prerequisites
1. Installed Winget. You can get winget from
   + [Microsoft Store](ms-windows-store://pdp/?ProductId=9nblggh4nns1)
   + [Manual Install](https://github.com/microsoft/winget-cli/releases/)
   + [Preinstalled on Windows 10 development environment](https://developer.microsoft.com/en-us/windows/downloads/virtual-machines/)

# Approach
The Windows Package Manager (aka winget) is a package manager from Microsoft for Windows.
Similiar to apt or yum, winget helps with the process of installing, upgrading and removing applications.
After installation, winget is run by the same name from a command prompt (cmd or powershell).

*Search for Applications with "winget search"*
{{< highlight cmd >}}
PS C:\Windows> winget search Chrome
Name                 Id                         Version       Match
-----------------------------------------------------------------------------
Google Chrome        Google.Chrome              92.0.4515.131 Moniker: chrome
Google Chrome Beta   Google.Chrome.Beta         93.0.4577.18  Command: chrome
Google Chrome Dev    Google.Chrome.Dev          93.0.4577.18  Command: chrome
Stack                stack.stack                3.32.0        Tag: chrome
Brave                BraveSoftware.BraveBrowser 92.1.27.109   Tag: Chrome
Ginger Chrome        Saxo_Broko.GingerChrome    93.0.4529.0
Google Chrome Canary Google.Chrome.Canary       94.0.4589.2
{{< / highlight >}}

*Install Applications with "winget install"*
{{< highlight cmd >}}
PS C:\Windows> winget install --id=Google.Chrome -e
Found Google Chrome [Google.Chrome]
This application is licensed to you by its owner.
Microsoft is not responsible for, nor does it grant any licenses to, third-party packages.
Downloading https://dl.google.com/edgedl/chrome/install/GoogleChromeStandaloneEnterprise64.msi
██████████████████████████████  76.2 MB / 76.2 MB
Successfully verified installer hash
Starting package install...
Successfully installed
{{< / highlight >}}

*winget install* can install a new browser without running Internet Explorer or Edge. 😋

With these commands you can create scripts to jump start your windows setup.
In order to install software silently use *-h* as a parameter and run winget in an elevated command prompt. 
Now the script run without the need for manual intervention.
I wrote some example scripts, which you can also find in 
[this GitHub project](https://github.com/torbenmoeller/setup-scripts).


**Example Script to install common tools**
{{< highlight powershell >}}
# Install tools that are support different use cases and are language and cloud-agnostic.
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

**Example Script to install programming tools**
{{< highlight powershell >}}
# Install tools for different programming languages
# Python
winget install --id=Python.Python.3 -e -h
winget install --id=JetBrains.PyCharm.Community -e -h
# Java
winget install --id=AdoptOpenJDK.OpenJDK.11 -e -h
winget install --id=JetBrains.IntelliJIDEA.Community -e -h
# Go
winget install --id=GoLang.Go -e -h
winget install --id=JetBrains.GoLand -e -h
{{< / highlight >}}

**Example Script to install cloud administration tools**
{{< highlight powershell >}}
# Install tools for cloud administration
# AWS
winget install --id=Amazon.AWSCLI -e -h
winget install --id=Amazon.SAM-CLI -e -h
# Azure
winget install --id=Microsoft.AzureCLI -e -h
winget install --id=Microsoft.AzureFunctionsCoreTools -e -h
# GCP
winget install --id=Google.CloudSDK -e -h
{{< / highlight >}}

# Further Reading
* Every script can be found in [this GitHub project](https://github.com/torbenmoeller/setup-scripts)
* In order to use the script after a fresh installation of Windows, you have to store them externally. 
  GitHub or an external hard drive are good solutions.
* Winget is open source, so share your feedback or contribute on [GitHub](https://github.com/microsoft/winget-cli) 
* [winstall.app](https://winstall.app/) is a helpful website to find software you may want to add to your scripts.
* Installing .zip files is [not yet supported](https://github.com/microsoft/winget-cli/issues/140),
  so tools like terraform, helm, maven have to be installed manually.
* [Chocolatey](https://chocolatey.org/) can be used together with winget or as an alternative.
  Chocolatey also supports installation of zip files. 
